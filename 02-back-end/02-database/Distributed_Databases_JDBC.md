---
title: 分布式数据库 JDBC
tags: [技术, 存储, 数据库]
updated: "2026-06-18"
---

# 分布式数据库 JDBC

## 简介
JDBC（Java Database Connectivity）提供一个标准 API 给 Java 应用程序使用关联关系数据库，包括分布式引擎。

## 关键主题
- **JDBC 驱动程**：Type 4（纯 Java）驱动程，适用于最新分布式数据库。
- **连接池**：HikariCP，Apache DBCP 等高并发性连接池。
- **SQL 方言**：对不同分布式数据库的方言差异进行处理（例如 CockroachDB，TiDB，YugabyteDB）。
- **事务管理**：XA 事务，二阶步接入，用于分片处理。
- **Failover 和负载均补**：多主机 JDBC URL、客户端自己负载均补。

## 实用示例
```java
String url = "jdbc:postgresql://db1:5432,db2:5432,db3:5432/mydb?targetServerType=primary&loadBalanceHosts=true";
try (Connection conn = DriverManager.getConnection(url, "user", "pass")) {
    // 执行 SQL 查询
}
```

## 性能推迟
- 使用凭识语句降低解析异常。
- 对大结果集设置合适的 fetchSize。
- 在支持的情况下开启客户端语句缓存。

## 参考资料
- JDBC 4.3 规范文档。
- CockroachDB JDBC 驱动程文档。
- 分布式 DB 环境里 Java 最佳工作建议。
