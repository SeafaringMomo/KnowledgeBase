---
title: Doris 概述
tags: [技术, 存储, 云]
updated: "2026-06-18"
---

# Doris 概述

## 简介
Apache Doris 是一个高性能实时分析数据库，专在报表和自意对象查询。

## 关键特性
- 列式存储及矢量执行。
- 分布式三尾比特尔安实并应用方框。
- 原生支持 MySQL 和 PostgreSQL 协议。
- 内置物理视图及双合计也可能。

## 架构概览
- **Frontend (FE)**：查询计划、元数据管理。
- **Backend (BE)**：存储、计算、数据复制。
- **Broker**：通过外部源导入/导出数据。

## 应用域
- 实时以数据盘（Dashboard）。
- 点击流数据分析。
- 需要下密度快速响应的业务智数分析。

## 与其他数据库对比
- vs ClickHouse：这两者都有列式引擎，Doris 更关注事分与 MySQL 兼容性。
- vs Druid：Doris 给出了带有复杂合并主数谱查询的 OLAP 机制。

## 开始使用
- 使用 Docker Compose 快速部署。
- 基本 SQL 语法和性能优化后端下提示。

## 参考资料
- Apache Doris 官方文档。
- 社区博帖上的性能调整活动。
