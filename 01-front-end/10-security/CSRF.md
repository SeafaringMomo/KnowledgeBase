# CSRF（跨站请求伪造）

## 攻击原理

用户已登录 A 网站，访问 B 网站时，B 网站诱导浏览器向 A 网站发送请求，A 网站误以为是用户本人操作。

---

## 防护措施

```html
<!-- 1. CSRF Token -->
<form method="POST" action="/transfer">
  <input type="hidden" name="csrf_token" value="随机值">
</form>
```

```http
<!-- 2. SameSite Cookie -->
Set-Cookie: session=xxx; SameSite=Strict; Secure; HttpOnly
```

```javascript
// 3. 验证 Origin/Referer
const allowedOrigin = 'https://example.com';
if (req.headers.origin !== allowedOrigin) {
  return res.status(403).send('Forbidden');
}
```

---

## XSS 与 CSRF 对比

| 攻击 | 窃取/做什么 | 核心防护 |
|------|-------------|----------|
| XSS | 执行恶意脚本、窃取数据 | 转义、CSP、HttpOnly |
| CSRF | 冒充用户发起请求 | Token、SameSite、Origin 校验 |

---

*Related: [[10-security/00-Index|安全]], [[10-security/XSS|XSS]]*
