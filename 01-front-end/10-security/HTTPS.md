# HTTPS

## 为什么需要 HTTPS

- **加密**：防止中间人窃听
- **完整性**：防止数据被篡改
- **身份认证**：确认服务器身份

---

## TLS/SSL 握手

```
客户端                          服务端
  | ---- ClientHello ------------> |
  | <--- ServerHello + 证书 ------- |
  | ---- 预主密钥（公钥加密） ----> |
  | <--- 完成握手 ---------------- |
  | ---- 加密通信 ----------------> |
```

---

## HSTS

强制浏览器使用 HTTPS 访问：

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

---

## 证书

- 使用 Let's Encrypt 等免费证书
- 确保证书链完整
- 定期更新，避免过期

---

*Related: [[10-security/00-Index|安全]], [[07-network/HTTP|HTTP]]*
