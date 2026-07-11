---
title: Spring Cloud Gateway RBAC（基于角色的访问控制）
tags: [技术, 安全, 云]
updated: "2026-06-18"
---

# Spring Cloud Gateway RBAC（基于角色的访问控制）

## 简介
Spring Cloud Gateway 为微服务提供灵活的 API 网关解决方案。本笔记介绍如何在网关中实现基于角色的访问控制（RBAC）。

## 主心概念
- **Route Definition（路由定义）**：path、header、query 等 **predicates** 与 **filters** 的组合。
- **Security Filters（安全过滤器）**：`AuthorizationFilter`、`AuthenticationFilter`。
- **Reactive Security（响应式安全）**：与 Spring Security WebFlux 的集成。
- **RBAC Model（角色‑权限模型）**：角色、权限以及资源映射关系。

## 实现步骤
1. 在 `pom.xml` 或 `build.gradle` 中加入 `spring-boot-starter-security` 与 `spring-cloud-starter-gateway` 依赖。
2. 定义 `SecurityWebFilterChain` Bean，配置为 OAuth2/JWT 资源服务器模式。
3. 编写自定义 `AuthorizationManager`，在路由元数据 (`metadata`) 中检查用户角色。
4. 在路由定义中使用 `metadata` 标记所需角色，例如 `requiredRoles: ["ADMIN"]`。
5. 将自定义 `AuthorizationManager` 注册到 `GatewayFilterSpec` 中，使其在过滤链中生效。

## 代码示例（Kotlin）
```kotlin
@Bean
fun securityWebFilterChain(http: ServerHttpSecurity): SecurityWebFilterChain {
    return http
        .authorizeExchange { exchanges ->
            exchanges.pathMatchers("/admin/**").hasRole("ADMIN") // 仅 ADMIN 角色可访问
                     .anyExchange().permitAll()               // 其它路径全部放行
        }
        .oauth2ResourceServer { it.jwt() } // 使用 JWT 进行身份校验
        .build()
}
```

## RBAC 测试
- 使用 `WebTestClient` 搭配包含角色声明的 Mock JWT Token 发起请求。
- 断言受保护路由返回 **401（未认证）** 或 **403（拒绝访问）**，未授权时不可访问。

## 参考资料
- Spring Cloud Gateway 官方文档。
- Spring Security Reactive 使用指南。
- OWASP 关于 API 网关安全的最佳实践。

*Last updated: 2026-06-18*