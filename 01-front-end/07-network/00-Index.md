---
title: 网络章节索引
tags: [index, 前端, 网络]
updated: "2026-06-19"
---

# 网络知识体系

## 📋 目录

- [[07-network/HTTP|HTTP]] — 协议、请求响应、状态码

---

## HTTP 协议

### 请求结构

```
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html
Accept-Language: zh-CN
```

### 响应结构

```
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234
Cache-Control: max-age=3600

<html>...</html>
```

---

## HTTP 方法

| 方法 | 语义 | 幂等 | 安全 |
|------|------|------|------|
| GET | 获取资源 | ✓ | ✓ |
| POST | 创建资源 | ✗ | ✗ |
| PUT | 更新（整体） | ✓ | ✗ |
| PATCH | 更新（部分） | ✗ | ✗ |
| DELETE | 删除 | ✓ | ✗ |
| HEAD | 获取头部 | ✓ | ✓ |
| OPTIONS | 预检 | ✓ | ✓ |

---

## 常见状态码

| 分类 | 范围 | 说明 |
|------|------|------|
| 1xx | 100-199 | 信息性 |
| 2xx | 200-299 | 成功 |
| 3xx | 300-399 | 重定向 |
| 4xx | 400-499 | 客户端错误 |
| 5xx | 500-599 | 服务端错误 |

### 常用状态码

| 状态码 | 说明 |
|--------|------|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 301 | 永久重定向 |
| 302 | 临时重定向 |
| 304 | Not Modified（缓存） |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 409 | Conflict |
| 422 | Unprocessable Entity |
| 429 | Too Many Requests |
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |
| 504 | Gateway Timeout |

---

## HTTP Headers

### 请求头

```
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html,application/json
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,en
Authorization: Bearer <token>
Cookie: session_id=xxx
Referer: https://google.com
Origin: https://example.com
Content-Type: application/json
Content-Length: 1234
```

### 响应头

```
Content-Type: text/html; charset=utf-8
Content-Length: 1234
Cache-Control: max-age=3600, public
Set-Cookie: session=xxx; HttpOnly; Secure; SameSite=Strict
ETag: "abc123"
Last-Modified: Wed, 04 Jun 2026 12:00:00 GMT
Location: /new-url
Access-Control-Allow-Origin: *
CORS 相关头
```

---

## 📚 资源

- [MDN HTTP](https://developer.mozilla.org/zh-CN/docs/Web/HTTP)
- [HTTP 状态码](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status)

---

*Last updated: 2026-06-19*

*Related: [[00-Index|前端知识体系]]*