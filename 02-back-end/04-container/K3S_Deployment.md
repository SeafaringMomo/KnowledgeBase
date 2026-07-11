---
title: K3s 部署
tags: [技术, 云, DevOps]
updated: "2026-06-18"
---

# K3s 部署

## 简介
K3s 是带有较小体量的 Kubernetes 引导版本，特别适合边缘、物联网及资源不太完善的环境。

## 部署方式
- **单节点**：可在虚拟机、物理服务器或小型平台上快速启动。
- **多节点 HA**：使用嵌入式 etcd 或外部数据库实现高可用性配置。
- **安装方法**：curl 脚本、Helm Chart 或 Ansible Playbook 都可用于自定义安装流程。

## 主要概念
- **Server 并程序**：控制层（指定环境配置）。
- **Agent 工作者**：工作节点，关联到 Server 并接收任务。
- **嵌入式组件**：containerd、flannel、CoreDNS 等快速容器和网络实时实现。
- **配置文件**：`k3s.yaml` 、环境变量、TLS 证书管理等，支持配置化和安全设置。

## 部署步骤
1. 安装 Docker 和 curl 等必需前置工具。
2. 执行 
   ```bash
   curl -sfL https://get.k3s.io | sh -
   ```
   安装 K3s。
3. 通过 `k3s kubectl get nodes` 或 `kubectl get nodes` 检查节点状态。
4. 部署一个示例应用，例如 Nginx Ingress 或 简单 网页服务。

## 运维提示
- 将 `k3s kubectl` 配置为 `kubectl` 别名，便于直接引用。
- 如果使用外部数据库，请在 控制台上安排 etcd 健康检查脚本。
- 为 HA 高可用配置备份：备份 "/var/lib/rancher/k3s/server/db" 目录下数据库。

## 参考资料
- K3s 官方文档，完整部署指南。
- 社区学习源，应用测试插件和扩展工具。

*Last updated: 2026-06-18*