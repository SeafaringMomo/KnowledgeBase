---
title: 容器与编排
tags: [技术, 容器, 编排]
updated: 2026-06-18
---

# 容器与编排

## 简要概述
本笔记收集了 Docker 和 Kubernetes 等容器技术及编排工具，包括 K3s 、 Helm 和 Kafka 的应用情况。

## 核心概念或技术要点
- **Docker**：轻量容器工具，支持容器建立、封装及处理应用。
- **K3s**：开源轻量版 Kubernetes，适合小型应用或转移应用。
- **Kubernetes (K8s)**：全面的容器编排平台，支持自动部署、过轻下发应用、服务自动治理等功能。
- **Kafka**：高速容器和计算数据流应用，支持异步处理和持续流序。
- **HTTP/Stream**：基于 HTTP 协议的数据流引擎，配合 Nginx 、Traefik 或 FastAPI 进行实时流引擎，让应用程序可以将数据流自带至前端。

## 常见使用场景
- **Docker**：开发环境、CI/CD 流程、封装上环境。
- **K3s**：小型服务流程、小路由器和边缘设备。
- **Kubernetes**：大型下分布式服务、应用资源管理、自动模块下发应用。
- **Kafka**：高速数据流运营、异步事件处理、异坐数据处理。
- **HTTP/Stream**：实时数据流应用，用于监控数据流和开发应用应用。

## 参考实现 / 常用工具
- **Docker**: `docker compose`, `Dockerfile`, `docker swarm`
- **K3s**: `k3s install script`, `helm chart` for K3s, `k3d` for local testing
- **Kubernetes**: `kubectl`, `helm`, `kustomize`, `k9s`
- **Kafka**: `kafka-topics.sh`, `kafka-console-producer.sh`, `kafka-streams`, `confluent platform`
- **HTTP/Stream**: `nginx`, `HAProxy`, `Traefik`, `FastAPI` streaming, `Node.js` `express` with `eventsource`
