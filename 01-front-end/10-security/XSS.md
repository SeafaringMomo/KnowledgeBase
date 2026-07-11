# XSS（跨站脚本攻击）

## 攻击原理

攻击者将恶意脚本注入到页面中，当其他用户访问时脚本执行，从而窃取 Cookie、伪造请求或篡改页面。

---

## 类型

| 类型 | 说明 |
|------|------|
| 存储型 XSS | 恶意内容被存储到服务器（如评论、文章） |
| 反射型 XSS | 恶意脚本通过 URL 参数反射到页面 |
| DOM 型 XSS | 前端 JavaScript 不安全地处理用户输入 |

---

## 防护措施

```javascript
// 1. 转义输出
function escapeHtml(str) {
  return str
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#39;');
}

// 2. 使用框架默认转义（React/Vue）
// 3. 不要直接用 innerHTML 插入不可信内容
// 4. 设置 HttpOnly Cookie
Set-Cookie: session=xxx; HttpOnly; Secure; SameSite=Strict

// 5. 配置 CSP
Content-Security-Policy: default-src 'self'; script-src 'self'
```

---

*Related: [[10-security/00-Index|安全]], [[10-security/CSP|CSP]]*
