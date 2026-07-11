# CSP（内容安全策略）

## 作用

CSP 通过白名单机制限制页面可以加载的资源来源，有效防止 XSS 与数据注入攻击。

---

## 配置方式

```http
Content-Security-Policy:
  default-src 'self';
  script-src 'self' https://trusted-cdn.com 'nonce-abc123';
  style-src 'self' 'unsafe-inline';
  img-src 'self' data: https:;
  connect-src 'self' https://api.example.com;
  font-src 'self' https://fonts.gstatic.com;
  frame-ancestors 'none';
  report-uri /csp-report;
```

```html
<!-- 也可通过 meta 标签配置，但不支持 report-uri -->
<meta http-equiv="Content-Security-Policy"
      content="default-src 'self'; script-src 'self';">
```

---

## 常用指令

| 指令 | 说明 |
|------|------|
| `default-src` | 默认资源来源 |
| `script-src` | JavaScript 来源 |
| `style-src` | CSS 来源 |
| `img-src` | 图片来源 |
| `connect-src` | fetch/XHR/WebSocket 来源 |
| `font-src` | 字体来源 |
| `frame-ancestors` | 允许嵌入当前页面的来源 |
| `report-uri` | 违规报告地址 |

---

*Related: [[10-security/00-Index|安全]], [[10-security/XSS|XSS]]*
