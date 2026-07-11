# 浏览器缓存机制

## 缓存类型

| 类型 | 位置 | 生命周期 | 用途 |
|------|------|----------|------|
| Service Worker | 浏览器独立线程 | 由代码控制 | PWA、离线缓存 |
| Memory Cache | 内存 | 页面关闭即消失 | 基础资源、图片 |
| Disk Cache | 磁盘 | 受 Cache-Control 控制 | 大部分静态资源 |
| Push Cache | HTTP/2 推送 | 会话期 | HTTP/2 服务端推送 |

---

## 强缓存

浏览器直接从本地读取资源，不发送请求。

```http
Cache-Control: max-age=31536000, immutable
Expires: Wed, 04 Jun 2027 12:00:00 GMT
```

---

## 协商缓存

强缓存失效后，浏览器携带缓存标识询问服务器是否可用。

```http
# 首次响应
Last-Modified: Wed, 04 Jun 2026 12:00:00 GMT
ETag: "abc123"

# 再次请求
If-Modified-Since: Wed, 04 Jun 2026 12:00:00 GMT
If-None-Match: "abc123"

# 未变化返回 304
HTTP/1.1 304 Not Modified
```

---

## 缓存优先级

```
Service Worker > Memory Cache > Disk Cache > Push Cache > 网络请求
```

---

## 推荐策略

| 资源 | 策略 |
|------|------|
| HTML | `Cache-Control: no-cache`（每次协商） |
| 构建产物（带 hash） | `max-age=31536000, immutable` |
| API 数据 | 按业务需求设置短缓存或协商缓存 |
| 图片/字体 | 长期强缓存 + 协商缓存 |

---

*Related: [[08-browser/00-Index|浏览器]], [[09-performance/Cache_Strategy|缓存策略]]*
