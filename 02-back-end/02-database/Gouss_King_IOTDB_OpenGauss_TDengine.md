---
title: Gouss、KingbaseES、IOTDB、OpenGauss 与 TDengine
tags: [技术, 存储, 物联网]
updated: "2026-06-18"
---

# Gouss、KingbaseES、IOTDB、OpenGauss 与 TDengine

## 简介
简要概述在物联网与分析场景中常用的四种时间序列及关系型数据库技术：Gouss、KingbaseES（简称 King）、Apache IOTDB、OpenGauss 与 TDengine。

## 数据库概述
- **Gouss**：轻量级嵌入式时间序列数据库，侧重低延迟写入。
- **KingbaseES**：企业级关系型数据库，严格遵循 SQL 标准，具备高可靠性。
- **Apache IOTDB**：开源时间序列数据库，针对层次化传感器数据进行优化。
- **OpenGauss**：华为开源的关系型数据库，支持 OLAP 与 OLTP 场景。
- **TDengine**：高性能时间序列数据库，内置压缩与数据生命周期管理功能。

## 共通特性
- 时间序列数据模型（时间戳 + 度量值）。
- 按时间区间进行分区（partition）。
- 压缩算法（如 LZ4、Gorilla）。
- 类 SQL 查询语言，提供时间函数支持。

## 选型标准
- **规模**：预期每日数据量。
- **查询复杂度**：简单聚合 VS 复杂关联查询。
- **生态系统**：客户端库、与 Grafana 等可视化工具的集成程度。
- **授权模式**：开源或商业授权。

## 集成模式
- 边缘设备采集 → IOTDB/TDengine → 批量导出至 OpenGauss 进行关系型分析。
- 使用 Apache Flink 在时间序列数据库与关系型存储之间进行流处理。

## 参考资料
- IOTDB、TDengine、OpenGauss 官方文档。
- 对比不同时间序列数据库性能的基准报告。

*Last updated: 2026-06-18*