---
title: Spark 与 Doris 框架
tags: [技术, 分析, 大数据]
updated: "2026-06-18"
---

# Spark 与 Doris 框架

## 简介
Apache Spark 与 Apache Doris 的集成提供了对流量大数据执行快速分析和查询的能力。

## 主要组件
- **Spark**：分布式数据处理引擎，支持 SQL，DataFrames 和 MLlib。
- **Doris**：列向分析数据库，提供高速查询服务。
- **Connector**：Spark 与 Doris 的连接器（JDBC 或 原生 JDBC 插件），用于数据读取和写入。

## 数据工作流程
1. 将原始数据存储到 HDFS 或 S3。
2. 使用 Spark 进行转换处理。
3. 通过 `spark-doris-connector` 将处理后的数据导入 Doris 表中（`append` 模式）。
4. 在 Doris 上直接运行查询，以优化数据获取的低延败延时间。

## 性能优化
- 批量写入时，每次将 10,000 以上行数的数据传输到 Doris。
- 开启 Doris `stream_load` 模式，便于连续影子流式数据轻松后入。
- 将 Spark 分区与 Doris 数据存储的 Tablet 分布兼容，使数据平均分布在多个 Node 上。
- 使用 Doris 列后叠字引擎与条件传递优化查询速度。

## 例程码（Scala）
```scala
val df = spark.read.format("csv").option("header","true").load("s3://bucket/data.csv")

df.write.format("doris")
  .option("doris.table.identifier", "mydb.mytable")
  .option("doris.fenodes", "doris-fe1:8030,doris-fe2:8030")
  .mode("append")
  .save()
```

## 应用域
- 实时分析代表板，汇总数据用于模块点数据展示。
- ETL 流程，将数据转换到 Doris 数据库。
- 基于 Doris 的机器学习异常检测，数据集合可用于经典模型。

## 参考资料
- Spark-Doris 连接器 GitHub 库。
- Apache Doris 最新文档，`Stream Load` 安装指南。

*Last updated: 2026-06-18*