# 点击劫持

## 攻击原理

攻击者将目标网站通过 iframe 嵌入到恶意页面，并覆盖透明层，诱导用户点击原本不想点击的按钮。

---

## 防护措施

```http
# 1. X-Frame-Options
X-Frame-Options: DENY
X-Frame-Options: SAMEORIGIN

# 2. CSP frame-ancestors
Content-Security-Policy: frame-ancestors 'none';
Content-Security-Policy: frame-ancestors 'self' https://trusted.com;
```

```javascript
// 3. JS 防御（辅助）
if (window.top !== window.self) {
  window.top.location = window.self.location;
}
```

---

*Related: [[10-security/00-Index|安全]], [[10-security/CSP|CSP]]*
