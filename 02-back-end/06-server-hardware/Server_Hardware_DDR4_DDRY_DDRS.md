---
title: 服务器硬件 DDR4 DDRY DDRS
tags: [技术, 硬件, 内存]
updated: "2026-06-18"
---

# 服务器硬件 DDR4 DDRY DDRS

## 简介
本笔记摘要介绍现代服务器内存 DDR4 及其变体 DDRY 和 DDRS 的特殊属性与应用情况。

## DDR4 基础知识
- 2400 到 3200 MT/s 标准颜色线，标准电压 1.2V。
- JEDEC 时间参数（CAS 延迟、tRCD、tRP等）当前固定。

## DDRY (DDR4‑Y)
- 专门用于低电子服务器，实现特殊的维少功能。
- 支持 on‑die termination 和动态频率缩小，提供较高特性性能。

## DDRS (DDR4‑S)
- 最大化内存模块密度，每个 DIMM 最大 32 GB。
- 支持 ECC 错误校正，强化服务器上数据存储的保存性。

## 配置指南
- 应使用配对序列字节连接 DIMM，开启双频数据传输。
- 选择符合 QVL（Qualified Vendor List）的内存库，使用支持的大平静速度。
- 在性能优化时，启用 XMP（Extreme Memory Profile）定义字节思运行，以解除并最大化性能。

## 故隐排除
- 使用 `dmidecode` 和 `memtest86+` 检查安装的内存模块。
- 检查 BIOS 设置，磁盘参数和时间对比是否兼容。
- 监即 ECC 错误日志，判断硬件故隐情况。

## 参考资料
- JEDEC DDR4 规范文档。
- 服务器硬件安装官方指南。

*Last updated: 2026-06-18*