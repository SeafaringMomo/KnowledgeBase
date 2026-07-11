---
title: Kubernetes 和 Docker
tags: [技术, 云, DevOps]
updated: "2026-06-18"
---

# Kubernetes 和 Docker

## 简介
Docker 提供容器运行时，Kubernetes 在大规模下组织容器。本笔记结合了两者之间的关系及其一般部署模式。

## 主要概念
- **Docker Engine**：形成影像、容器生命周期，Dockerfile 语法。
- **Kubernetes**：pod、deployment、service、ingress、StatefulSet 等组件。
- **Container Runtime Interface (CRI)**：Kubernetes 与 Docker（或 containerd 等其他容器运行时的交互方式。
- **Image Registry**：Docker Hub、私有注册中心，在 pod 中拉取影像。

## 工作流程
1. 定义 `Dockerfile` 并把影像构建。
2. 将影像上传到 Registry。
3. 在 Kubernetes YAML 中定义 `Deployment` 引用这个影像。
4. 使用 `kubectl apply` 应用这些 yaml 文件。
5. 使用 `Service` 并可选 `Ingress` 开放外部访问。

## 最佳工作提示
- 使用多阶段构建（multi‑stage build）减小影像大小。
- 部署前对影像进行安全性检测。
- 为私有 Registry 使用 Kubernetes `imagePullSecrets` 配置凭证。
- 为 pod 设置资源请求和限制（resource requests/limits）。

## 故 障排除提示
- 使用 `kubectl logs` 查看容器日志。
- 使用 `kubectl exec` 进入正在运行的容器。
- 检查节点版本中 Docker daemon 状态。

## 参考文档
- Docker 官方文档。
- Kubernetes 官方概念指南。

*Last updated: 2026-06-18*