---
title: CDC 增量数据捕获
tags: [技术, 数据库, 数据工程]
updated: "2026-11-24"
---

# CDC 增量数据捕获

## 简介
CDC（Change Data Capture）从数据库的**日志**里读取行级变更（insert / update / delete），而不是查询业务表。它解决的是"源库的变更如何低侵入、增量、有序地流到下游"。

常被描述成"高并发下比轮询有优势"——结论对，但理由容易记歪：**CDC 的优势不是更快，而是更省**。源库再忙，CDC 只读日志，几乎不产生查询压力。

## 核心机制

```
业务写入 → 数据库日志（MySQL binlog / PG WAL / Oracle redo）
        → CDC 连接器解析日志（Debezium / Canal / Flink CDC）
        → 变更事件流（含变更前后镜像 + 位点）
        → 下游：数仓 / 缓存 / 搜索 / 消息队列
```

三个关键性质：

1. **行级**：拿到变更前后的完整行镜像，不只是"这行变了"。
2. **有序**：同一主键的变更按提交顺序到达（单分区内）。
3. **带位点**：消费进度可记录，支持断点续传。

## 与其它同步方式的对照

| 方式 | 对源库压力 | 能否捕获删除 | 延迟 | 主要问题 |
|---|---|---|---|---|
| 定时轮询 `WHERE update_time > ?` | 随频率线性上升 | **不能**（物理删除查不到） | 一个轮询周期 | 同周期内多次更新只看到最后一次 |
| 触发器写变更表 | 高（写入放大约 2 倍） | 能 | 低 | 侵入业务，DDL 维护麻烦 |
| 应用双写 | 无 | 能 | 最低 | 代码侵入、易漏写、无事务保证 |
| **CDC 读日志** | **几乎为零** | 能 | 毫秒 ~ 秒 | 依赖日志配置与权限 |

## 成本与失败模式

| 失败模式 | 原因 | 应对 |
|---|---|---|
| 断点补不回来 | binlog 被 purge，位点已失效 | 监控日志保留时长；位点落后告警 |
| DDL 变更打穿下游 | 加列 / 改类型未同步 schema | schema 演进流程 + 兼容性检查 |
| 大事务打爆下游 | 一次百万行 UPDATE 形成巨量事件 | 限流、背压、按主键分片 |
| 无主键表无法幂等 | 无法定位行 | 强制业务表有主键；否则降级为全表对比 |
| 重复消费 | 至少一次投递语义 | 下游按（主键 + 位点 / LSN）幂等写入 |
| 快照与增量衔接错位 | 全量快照与增量起点不一致 | 用一致性快照（如 Debezium incremental snapshot） |

> **Exactly-once 不是 CDC 单方面能给的**，必须端到端：幂等写 + 位点与数据在同一事务内提交。

## 在数据链路中的位置

CDC 属于**采集层**，不是展示层。典型分层：

```
业务库（OLTP）
   ↓ CDC
消息队列（Kafka）—— 缓冲、解耦、可回放
   ↓
数仓 / 列存 OLAP（Doris、ClickHouse）
   ↓
BI / 大屏（只读查询）
```

**大屏不该直连 CDC 流**：前端要承担流式去重、乱序处理、状态管理，成本和风险都不划算。大屏应消费"CDC 落库后的宽表或聚合表"。

## 实践建议

1. **先确认日志可用性**：MySQL 需 `binlog_format=ROW` 且 `binlog_row_image=FULL`；PostgreSQL 需 `wal_level=logical` 并建逻辑复制槽。
2. **日志保留时长 ≥ 最长故障恢复时间**，否则一次下游宕机就要重做全量。
3. **给 CDC 单独账号**，只授 `REPLICATION SLAVE` / `REPLICATION CLIENT`，不要复用业务读写账号。
4. **监控三个指标**：位点延迟、事件速率、快照 / 增量衔接点。
5. 下游建表时**先设计幂等键**，再谈性能。

## 参考资料

- [Debezium 官方文档](https://debezium.io/documentation/)
- [MySQL Binary Log 说明](https://dev.mysql.com/doc/refman/8.0/en/binary-log.html)
- [PostgreSQL 逻辑复制](https://www.postgresql.org/docs/current/logical-replication.html)
- 相关笔记：[HTAP 与数据库负载模型](HTAP_and_Workload_Models.md)、[Doris 概述](Doris_Overview.md)

*Last updated: 2026-11-24*
