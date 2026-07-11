---
title: SQL 注入和 JWT 安全论计
tags: [安全, 技术, 网页]
updated: "2026-06-18"
---

# SQL 注入和 JWT 安全论计

## 简介
本章调查两个关键的 Web 安全论题：SQL 注入风险和 JSON Web Token（JWT） 基于认证的验证。

## SQL 注入
- **定义**：引擎通过插入消防的输入数据对 SQL 语句进行驗点块。
- **常见向量**：WHERE 子句中用户参数、动态串联语句编译等。
- **预防措施**
  - 使用准备语句或参数化查询。
  - 应用 ORM 框架可自动封装 SQL，减少农转相关风险。
  - 对输入进行验证和清除。
  - 使用最小权限数据库帐号。
- **测试法**：使用 sqlmap，OWASP ZAP 等工具进行安全测试。

## JWT 认证
- **结构**：Header，Payload，Signature（Base64URL 编码）。
- **应用域**：非状态会话标记，API 认证。
- **安全考虑**
  - 选择强强算法（HS256 与 secret，RS256 与 private key）。
  - 设置合适的过期时间（`exp` claim）。
  - 保存令牌的安全性：httpOnly 定位 cookie，避免用 localStorage 应对 XSS。
  - 实施撤锁策略：token blacklist，短 TTL。
- **常见环罪**
  - 使用 none 算法。
  - 不检证 Signature。
  - 在 Payload 中嵌入敏感数据。

## 合同风险
- 在生成 Token 的接口中同时安全处理 SQL 注入。
- 使用 JWT 库时，应用 Parameterized Query 将 Claim 存储到数据库。

## 参考资料
- OWASP Top 10 (A01: Injection, A07: Identification & Authentication)
- RFC 7519 (JSON Web Token)
- sqlmap 文档

*Last updated: 2026-06-18*