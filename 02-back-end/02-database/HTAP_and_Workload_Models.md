---
title: HTAP 与数据库负载模型（OLTP / OLAP / HTAP）
tags: [技术, 数据库, 架构]
updated: "2026-11-24"
---

# HTAP 与数据库负载模型（OLTP / OLAP / HTAP）

## 简介
数据库按"承担什么负载"分为三类：**OLTP**（联机交易）、**OLAP**（联机分析）、**HTAP**（混合事务分析处理）。HTAP 不是新协议，也不是新查询语言，而是一种系统定位——用同一套系统同时服务毫秒级点查/点写和秒级大范围聚合。

> **与 HTTP 无关。** 两个缩写只是撞脸：HTTP = **HyperText** Transfer Protocol（L7 应用层协议），HTAP = **Hybrid** Transactional/Analytical Processing（负载模型）。HTAP 数据库照样用 HTTP 对外提供接口（OceanBase 的 HTTP API、Doris 的 Stream Load）。一个是"数据怎么送过来"，一个是"数据库自己怎么算"，层级不同，不能对比。

## 三种负载的对照

| 维度 | OLTP | OLAP | HTAP |
|---|---|---|---|
| 典型语句 | `SELECT * FROM t WHERE id=?`、`UPDATE ... WHERE id=?` | `SELECT unit_id, SUM(x) FROM t WHERE ts BETWEEN ? AND ? GROUP BY unit_id` | 两者混合 |
| 单次触及数据 | 几行 | 百万 ~ 亿行 | 按负载路由 |
| 响应目标 | 毫秒 | 秒 ~ 分钟 | TP 毫秒 / AP 秒级 |
| 并发 | 极高（万级 QPS） | 低（几个大查询） | 混合，必须资源隔离 |
| 存储形态 | 行存 | 列存 | 行存 + 列存副本 |
| 主要瓶颈 | IOPS、锁竞争、事务日志 | CPU、扫描带宽 | **资源争抢** |
| 代表产品 | MySQL、PostgreSQL、OceanBase | ClickHouse、Doris、Greenplum | OceanBase、TiDB + TiFlash、SAP HANA |
| 一句话 | 记账 | 算账 | 边记账边算账 |

### 为什么会有这个分野：行存 vs 列存

- **行存**把一行连续存放，取整行快（`WHERE id=?` 命中一条记录），适合交易。
- **列存**把一列连续存放，同列类型一致、压缩率高、只读需要的列，适合 `SUM / AVG / GROUP BY`。

这是**物理层冲突**，不是调参能消除的。HTAP 要解决的核心问题就是它。

## HTAP 的两条实现路线

### 路线一：一份数据、多副本不同形态（真 HTAP）

主副本行存服务 TP，另建**列存副本**服务 AP，同一事务引擎保证一致。

- OceanBase 4.3+ 的列存副本
- TiDB 的 TiFlash（行存 TiKV + 列存 TiFlash）

优点：无 ETL、数据不重搬、延迟低。
代价：**列存副本异步同步**（秒级延迟）、写放大（一份数据写两遍）、存储成本上升。

### 路线二：双系统 + CDC 同步（业内常说的"伪 HTAP"）

MySQL 扛交易，CDC 把变更推到 Doris/ClickHouse 扛分析。

优点：各用最合适的引擎、互不干扰、成本低。
代价：分钟级延迟、多一套运维、schema 要同步演进。

> 识别话术：选型时问一句"**列存是同一份数据，还是另一套副本？延迟多少？**"即可分辨路线一与路线二。

## 成本与失败模式

| 失败模式 | 原因 | 表现 |
|---|---|---|
| 分析拖垮交易 | AP 大查询抢占 CPU/IO | TP 的 P99 抖动，业务超时 |
| 资源隔离不彻底 | 只做 SQL 层限流，没做物理隔离 | 高峰期一张报表把交易库拖死 |
| 优化器两头不讨好 | 一套优化器兼顾点查与聚合 | 点查不如纯 OLTP，聚合不如纯列存 |
| 列存副本延迟被忽略 | 异步复制 | 刚提交的数据在报表里查不到，被当成 bug |
| 为偶发需求付分布式代价 | 一天 20 个报表却上多副本集群 | 运维复杂度是纯 OLAP 的三倍 |

## 选型判断

| 场景 | 选择 |
|---|---|
| 分析必须与交易强一致（实时风控、实时对账、实时额度） | HTAP |
| 分析可容忍分钟级延迟（绝大多数场景） | OLTP + CDC + 列存 OLAP |
| 数据量小、运维人力只有一两个人 | HTAP（少维护一套） |
| 纯报表 / BI / 大屏历史趋势 | OLAP（Doris 足够） |
| 只存当前值、秒级刷新 | Redis，不查库 |

**判断口诀：分析结果晚 1 分钟会不会造成损失？会 → HTAP；不会 → 双系统。**

## 实践建议

1. **先证明需要强一致**，再考虑 HTAP。绝大多数"实时分析"其实是"准实时"。
2. 上 HTAP 必须同时上**资源隔离**（OceanBase 用租户 / Resource Unit，TiDB 用独立 TiFlash 节点），否则等于把分析负载灌进交易库。
3. 明确列存副本的**同步延迟**并写进 SLA，避免把"查不到刚写入的数据"当 bug 排查。
4. 大屏、报表这类**读多写零**的消费端属于 OLAP 需求，不要用 HTAP 架构去满足。

## 参考资料

- [Apache Doris 官方文档](https://doris.apache.org/zh-CN/docs/)
- [OceanBase 产品文档](https://www.oceanbase.com/docs)
- [PostgreSQL 官方文档](https://www.postgresql.org/docs/current/)
- 相关笔记：[Doris 概述](Doris_Overview.md)、[分析型数据库](Analytical_Databases.md)、[OceanBase 概述](OceanBase_Overview.md)、[CDC 增量数据捕获](CDC_Incremental_Data_Capture.md)

*Last updated: 2026-11-24*
