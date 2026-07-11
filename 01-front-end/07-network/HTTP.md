# 网络基础

## TCP 三次握手

```
客户端 ────────────────────────────── 服务端

       ──── SYN=1, seq=x ──────────→
       
←────── SYN=1, ACK=1, seq=y, ack=x+1 ─

       ──── ACK=1, seq=x+1, ack=y+1 ─→

              连接建立
```

### 为什么需要三次？

1. **第一次握手**：客户端发送请求，服务端确认"客户端能发"
2. **第二次握手**：服务端响应，客户端确认"服务端能收能发"
3. **第三次握手**：客户端再次确认，服务端确认"客户端能收"

---

## TCP 四次挥手

```
客户端 ────────────────────────────── 服务端

       ──── FIN=1, seq=u ──────────→
       
←────── ACK=1, ack=u+1 ─────────────

←────── FIN=1, ACK=1, seq=w ───────

       ──── ACK=1, ack=w+1 ─────────→

              连接断开
```

### TIME_WAIT

- 等待 2MSL（Maximum Segment Lifetime）确保对方收到最后的 ACK
- 防止旧连接的延迟数据包被新连接接收

---

## HTTP 缓存

### 强缓存

```
Cache-Control: max-age=3600        # 3600秒内不再请求
Expires: Wed, 04 Jun 2026 12:00:00 GMT  # 绝对时间（优先级低）
```

### 协商缓存

```
# 首次请求
Last-Modified: Wed, 04 Jun 2026 12:00:00 GMT

# 后续请求
If-Modified-Since: Wed, 04 Jun 2026 12:00:00 GMT
# 服务端检查，如未变化返回 304
```

### ETag

```
# 首次请求
ETag: "abc123"

# 后续请求
If-None-Match: "abc123"
# 服务端匹配，如一致返回 304
```

### 缓存优先级

```
1. no-cache      → 强制协商缓存
2. no-store      → 不缓存
3. private       → 仅浏览器缓存
4. public        → 可被 CDN 等中间节点缓存
5. max-age       → 强缓存
```

---

## CORS 跨域

### 简单请求

```
Origin: https://example.com

Access-Control-Allow-Origin: *  # 或具体域名
```

### 预检请求 (Preflight)

```
OPTIONS /api HTTP/1.1
Origin: https://example.com
Access-Control-Request-Method: DELETE

# 响应
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, DELETE
Access-Control-Allow-Headers: Content-Type
Access-Control-Max-Age: 86400
```

### 带 Cookie 的请求

```javascript
fetch(url, {
  credentials: "include"  // 发送 Cookie
});
```

```
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: https://example.com  # 不能是 *
```

---

## WebSocket

### 握手

```javascript
// 客户端
const ws = new WebSocket("wss://example.com/socket");

ws.onopen = () => {
  ws.send("Hello");
};

ws.onmessage = (event) => {
  console.log(event.data);
};
```

```
# HTTP 升级请求
GET /socket HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13

# 响应
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: base64_code
```

---

## HTTP/2 vs HTTP/3

| 特性 | HTTP/1.1 | HTTP/2 | HTTP/3 |
|------|----------|--------|--------|
| **多路复用** | 管线化(pipelining)有限 | ✓ 流复用 | ✓ 基于 UDP |
| **头部压缩** | 无 | HPACK | QPACK |
| **服务器推送** | 无 | ✓ | ✓ |
| **加密** | 可选 | 强制 TLS | 强制 QUIC |
| **队头阻塞** | 有 | 有（TCP 层） | 无 |

---

*Related: [[07-network/00-Index|网络]]*