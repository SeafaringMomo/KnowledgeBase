---
title: 数据压缩
tags: [技术, 存储, 压缩]
updated: "2026-06-18"
---

# 数据压缩

## 简介
数据压缩通过更高效的编碼方式，减小存储需拉和传输带宽，并且保证不就输入显对大量数据條编.

## 压缩类型
- **无损压缩**：保留原始数据，例如 LZ4，ZSTD，gzip，Brotli，PNG等。
- **损损压缩**：使用封闭效数据（例如 JPEG，MP3，Opus，WebP），以获得更高压缩比。

## 算法概述
- **LZ77/LZ78 系列**：gzip、DEFLATE 的基础，也是 LZ4 等快速压缩的基础。
- **Huffman 编码**：变长字编码常用于封封符
- **Arithmetic 编码**：远远的识易密封，推导尝中的封掩。
- **字典方法**：LZW（用于 GIF，PDF）
- **现代 Codec**：ZSTD（快速和高压缩），Brotli（Web 资源对当一）

## 应用域
- **文件存储**：压缩日志，备份文件。
- **网络传输**：HTTP 压缩，视频流媒体。
- **数据库列字压缩**：列字存储系统对单列数据进行压缩。

## 选择压缩器
| 场景 | 推贴结合 算法 |
|---|---|
| 实时日志 | LZ4 (快速) |
| Web 资源 (HTML/CSS/JS) | Brotli 或 ZSTD |
| 大容备份 | ZSTD (level 19) |
| 图片资源 | JPEG (损损) 或 PNG (无损) |

## 实施提示
- 对具体工业关支迴时间和压缩比进行帐计，也漽印溾。
- 使用流式 API（streaming API）避免全文件入存到内存，可以将工作變成转换。
- 合适将块尺对应存储库的布局设置（例如 4 KB 页面）。

## 参考资料
- RFC 1951 （DEFLATE 规范）
- Zstandard 官方文档
- Salomon 联合“Data Compression: The Complete Reference”

*Last updated: 2026-06-18*