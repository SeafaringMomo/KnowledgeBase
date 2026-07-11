---
title: 系统运用
tags: [技术, 系统]
updated: "2026-06-18"
---

# 系统运用

## 简要概述
本笔记分析了系统运用中常见的工具和组件，包括 RBAC、Gateway、Polphin Schedules、Hermes 图书管理接入、TSI/TOM、DCS/ICS 等。

## 核心概念或技术要点
- **RBAC**：基于角色的访问控制，用于管理系统权限。
- **Gateway**：在资源平台中进行请求过滤和调度。
- **Polphin Schedules**：一种任务调度方式，可使用 `cron`、`Airflow`、`Quartz` 等实现。
- **Hermes 图书管理接入**：通过 API、Webhook、RESTful 服务实现数据接入和控制。
- **TSI/TOM**：时序数据与模型管理平台，如 Azure Time Series Insights、Grafana TSI 插件。
- **DCS/ICS**：工业控制与监控系统，如 SCADA、Ignition、OPC UA。

## 常见使用场景 / 优势 / 限制
- **RBAC**：适用于细粒度权限管理，需维护角色定义和策略。
- **Gateway**：用于统一入口和安全防护，配置复杂度随流量规模提升。
- **Polphin Schedules**：适合定时任务和工作流编排，需考虑任务依赖与容错。
- **Hermes 接入**：帮助统一图书资源管理，需处理数据一致性。
- **TSI/TOM**：实时监控与分析时序数据，需关注存储成本。
- **DCS/ICS**：关键工业控制，强调安全与可靠性。

## 参考实现 / 常用工具
- **RBAC**：Keycloak、Auth0、OPA、Kubernetes RBAC
- **Gateway**：NGINX、Traefik、Kong、AWS/Azure API Gateway
- **Polphin Schedules**：cron、Airflow、Quartz
- **Hermes 接入**：Hermes API、Webhook、RESTful
- **TSI/TOM**：Azure Time Series Insights、Grafana TSI 插件、OpenTSDB
- **DCS/ICS**：SCADA、Ignition、OPC UA、Mirth Connect

*Last updated: 2026-06-18*