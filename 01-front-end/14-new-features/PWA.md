# PWA（渐进式 Web 应用）

## 核心特性

- **可安装**：像原生应用一样添加到主屏幕
- **离线访问**：Service Worker 缓存资源
- **推送通知**：通过 Push API 接收消息
- **响应式**：适配各种设备

---

## 核心技术

| 技术 | 作用 |
|------|------|
| Service Worker | 离线缓存、后台同步、推送 |
| Web App Manifest | 应用名称、图标、主题色、启动方式 |
| HTTPS | Service Worker 要求安全上下文 |

---

## Web App Manifest

```json
{
  "name": "My App",
  "short_name": "App",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#fff",
  "theme_color": "#007bff",
  "icons": [
    { "src": "/icon-192.png", "sizes": "192x192" },
    { "src": "/icon-512.png", "sizes": "512x512" }
  ]
}
```

```html
<link rel="manifest" href="/manifest.json">
```

---

## Workbox

Google 提供的 Service Worker 库，简化缓存策略。

```bash
npm install workbox-build
```

```javascript
// service-worker.js
import { precacheAndRoute } from 'workbox-precaching';
precacheAndRoute(self.__WB_MANIFEST);
```

---

*Related: [[14-new-features/00-Index|新技术]], [[08-browser/Cache_Mechanism|浏览器缓存]]*
