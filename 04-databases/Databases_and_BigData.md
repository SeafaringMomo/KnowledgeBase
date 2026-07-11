---
title: 数据库与大数据
tags: [技术, 数据库, 大数据]
updated: 2026-06-18
---

# 数据库与大数据

## 简要概述
本笔记整理了常见的数据库与大数据技术，包括关系型数据库、分析型数据库、时序数据库以及大数据计算框架等。

## 核心概念或技术要点
- **JDBC**：Java 数据库连接的标准 API，支持 MySQL、PostgreSQL、Oracle、OpenGauss 等。
- **分析型数据库**：用于大规模数据分析的数据库，如 ClickHouse、Druid、Doris 等。
- **OpenGauss**：国产开源关系型数据库，具备高安全性和高可用性。
- **TDengine**：专为时间序列数据设计的高性能数据库。
- **Spark**：分布式计算框架，支持批处理、流处理和机器学习。
- **Doris**：面向实时分析的 MPP 数据库，兼具高吞吐和低查询延迟。
- **IoTDB**：物联网时间序列数据库，提供高效写入和查询能力。

## 常见使用场景
- **JDBC**：Java 应用的数据库访问，常配合 HikariCP、MyBatis、Spring Data JPA 使用。
- **分析型数据库**：实时 OLAP 场景，如日志分析、业务监控。
- **OpenGauss**：企业级业务系统的核心数据库。
- **TDengine**：IoT、监控、日志等高频写入的场景。
- **Spark**：大规模离线批处理、机器学习模型训练。
- **Doris**：实时报表和交互式查询。
- **IoTDB**：物联网设备数据的存储与查询。

## 参考实现 / 常用工具
- **JDBC**: `java.sql.Connection`, `HikariCP`, `MyBatis`, `Spring Data JPA`
- **OpenGauss**: `psql`, `openGauss-tools`, `pgAdmin`
- **TDengine**: `taos` CLI, `taosAdapter` for Grafana, Java/Python SDK
- **Spark**: `spark-submit`, `pyspark`, `spark-sql`, `spark-streaming`
- **Doris**: `doris-fe` + `doris-be` 集群, `doris-sql` 客户端, `Flink` 连接器
- **IoTDB**: `iotdb-cli`, `IoTDB` Java SDK, `Grafana` 数据源插件
