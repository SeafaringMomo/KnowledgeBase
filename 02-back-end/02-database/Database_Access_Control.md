---
title: 数据库访问控制（表级 / 列级 / 行级权限）
tags: [技术, 数据库, 安全]
updated: "2026-11-24"
---

# 数据库访问控制（表级 / 列级 / 行级权限）

## 简介
数据库权限有五个递进的粒度：**实例/集群 → 库 → 表 → 列 → 行**。业务上最常被混淆的是"表级"和"行级"：

- **表级权限**回答"你能不能访问这张表"——全有或全无。
- **行级权限**（Row-Level Security，RLS；部分 BI 工具叫"行集权限""数据权限"）回答"同一张表里，你能看到哪些记录"。

一句话记：**表级是门禁卡，行级是进门后只能走自己那条走廊。**

## 三种粒度的对照

| | 表级 | 列级 | 行级 |
|---|---|---|---|
| 控制对象 | 对象（表） | 对象（列） | 数据内容（行） |
| 语义 | 能否访问该表 | 能访问哪些列 | 能看见哪些记录 |
| 实现 | `GRANT`（所有库原生） | `GRANT SELECT(col...)` | RLS / 视图 / 应用层 WHERE / 代理改写 |
| 绕过难度 | 低（DB 强制） | 低（但 `SELECT *` 会直接报错） | **高（视图与应用层都能绕）** |
| 性能影响 | 无 | 无 | **有**（谓词注入，可能破坏索引选择） |

## 表级 / 列级：GRANT 就够了

```sql
-- 库级
GRANT SELECT ON plant.* TO 'screen_ro'@'%';
-- 表级
GRANT SELECT ON plant.unit1_realtime TO 'screen_ro'@'%';
-- 列级（MySQL / PostgreSQL / Doris 均支持）
GRANT SELECT(alarm_code, alarm_time, level) ON plant.alarms TO 'screen_ro'@'%';
-- 回收
REVOKE SELECT ON plant.unit1_realtime FROM 'screen_ro'@'%';
```

Doris 的层级写法是 `ctl.db.tbl`：全局 `*.*.*` → Catalog `ctl.*.*` → 库 `ctl.db.*` → 表 `ctl.db.tbl`。

> 列级权限的副作用：授了列权限后 `SELECT *` 会直接报错，依赖全列查询的 ORM 会失效。

## 行级：四种实现

### 方案 A：数据库原生 RLS（最可靠）

| 数据库 | 支持情况 | 语法要点 |
|---|---|---|
| PostgreSQL | 9.5+ | `ALTER TABLE ... ENABLE ROW LEVEL SECURITY` + `CREATE POLICY` |
| SQL Server | 2016+ | `CREATE SECURITY POLICY` + 内联表值函数；分 filter（静默过滤）与 block（显式拒绝写） |
| Oracle | VPD（`DBMS_RLS`），**仅企业版** | 策略函数返回 WHERE 串 |
| Doris | **1.2+** `CREATE ROW POLICY`（社区版可用） | `AS RESTRICTIVE`（多策略 AND）/ `PERMISSIVE`（OR） |
| OceanBase | **仅企业版**有行级标签安全 | 社区版无此能力 |
| MySQL | **无原生 RLS** | 只能靠视图或应用层 |

PostgreSQL：

```sql
ALTER TABLE unit_realtime ENABLE ROW LEVEL SECURITY;

CREATE POLICY unit_isolation ON unit_realtime
  FOR SELECT TO screen_ro
  USING (unit_id = (SELECT current_setting('app.unit_id')::int));
```

Doris：

```sql
CREATE ROW POLICY p_unit1 ON plant.unit_realtime
  AS RESTRICTIVE TO role_unit1
  USING (unit_id = 1);
-- 多条 RESTRICTIVE 之间 AND，PERMISSIVE 之间 OR
SHOW ROW POLICY;
```

限制：Doris 的 `root` / `admin` 不受 Row Policy 约束（官方明确说明）。

### 方案 B：视图法（兼容性最好，但是"软边界"）

```sql
CREATE VIEW v_unit1 AS SELECT * FROM unit_realtime WHERE unit_id = 1;
GRANT SELECT ON v_unit1 TO role_unit1;
-- 关键：必须收回基表权限，否则直接查基表就绕过
REVOKE ALL ON unit_realtime FROM role_unit1;
```

**不易察觉的坑**：PostgreSQL 中普通视图的谓词是**普通 qual**，优化器可能把用户条件挪到过滤之前。若用户传入非 `LEAKPROOF` 的自定义函数，函数可能在过滤前就拿到**其它租户的行**（侧信道泄露）。

修法是 `CREATE VIEW ... WITH (security_barrier)`，代价是**优化器不能下推、索引可能失效**。PostgreSQL 15+ 还可用 `security_invoker = true` 让视图遵守底层表的 RLS。

> 业内共识：**"普通视图比 RLS 快，是因为它承诺得更少。"**

### 方案 C：应用层拼 WHERE（最常见，也最危险）

```python
sql = "SELECT * FROM unit_realtime WHERE unit_id = %s" % user.unit_id
```

- 优点：灵活、性能最好、能表达 RLS 写不出的业务逻辑（如"仅工作时段可见"）
- 缺点：**漏一处就完全失效**。查询入口越多，风险越大

### 方案 D：网关 / 代理改写

Ranger、ShardingSphere、MyCat 等在中间层重写 SQL。集中收口，但多一层运维对象，且必须确保所有流量都经过它。

## 性能：RLS 不是"查完再过滤"

RLS 把策略表达式作为 **security qual 注入执行计划**，不是先全查再筛。简单等值谓词**能走索引**：

```
Bitmap Heap Scan on orders
  Recheck Cond: (tenant_id = (InitPlan 1).col1)
  -> Bitmap Index Scan on orders_tenant_idx
```

三类情况会退化：

| 情况 | 后果 |
|---|---|
| 策略里用 `LIKE` 或自定义函数（非 LEAKPROOF） | 不能下推到索引前，退化为顺序扫描 |
| 策略里带子查询 | 变成独立 subplan，无法 flatten 成 join，优化器失去重排空间 |
| 同表多条策略 | 每行都要过一遍组合表达式，成本叠加 |

优化：把不依赖行数据的函数包成子查询，让数据库只算一次：

```sql
USING (unit_id = (SELECT current_setting('app.unit_id')::int))
```

## 三个必须知道的绕过口

1. **PostgreSQL 中超级用户、`BYPASSRLS` 角色、表 owner 直接绕过策略**（owner 需 `FORCE ROW LEVEL SECURITY` 才受约束）。
2. **`TRUNCATE` 不看策略**——一次 TRUNCATE 把多租户数据全清，策略毫无作用。
3. **Doris 的 `root` / `admin` 不受 Row Policy 约束**。

## 组合原则

正确姿势是三层叠加，缺一不可：

```
GRANT      → 能不能进这个房间（表级 / 列级）
RLS        → 能看哪几行（行级）
脱敏/掩码  → 哪几列要打码（列级 + 数据脱敏）
```

## 实践建议

1. **先判断是否真需要行级**。只读、单一数据源、固定账号的系统（如大屏展示端）用"表级只读 + 视图"就够，上 RLS 只是增加故障点。
2. 需要行级的典型信号：多机组 / 多部门共用一套后端，或产品化交付给多个客户（多租户）。
3. **优先在数据库层做，而不是应用层**。查询入口一多（配置页、历史查询、导出、API、下钻），应用层拼 WHERE 一定会漏。
4. 特别注意三类旁路：**导出功能**（常走另一套 SQL）、**缓存**（key 未带租户维度会串数据）、**截图 / 投屏**（看得见就带得走）。
5. 上线前做一次"换账号验证"：用受限账号跑一遍所有查询入口，而不是只测主页面。

## 参考资料

- [PostgreSQL Row Security Policies](https://www.postgresql.org/docs/current/ddl-rowsecurity.html)
- [SQL Server 行级安全性](https://learn.microsoft.com/zh-cn/sql/relational-databases/security/row-level-security)
- [Apache Doris 数据访问控制](https://doris.apache.org/zh-CN/docs/dev/admin-manual/auth/authorization/data/)
- [Oracle VPD 文档](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/using-oracle-vpd-to-control-access-to-data.html)
- 相关笔记：[HTAP 与数据库负载模型](HTAP_and_Workload_Models.md)、[OceanBase 概述](OceanBase_Overview.md)

*Last updated: 2026-11-24*
