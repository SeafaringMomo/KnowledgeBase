# 缓存策略

## 静态资源缓存

带 hash 的构建产物可以使用长期强缓存：

```http
Cache-Control: public, max-age=31536000, immutable
```

---

## HTML 与 API 缓存

```http
# HTML 使用协商缓存
Cache-Control: no-cache

# API 按业务设置
Cache-Control: private, max-age=300
```

---

## Service Worker 缓存

```javascript
// service-worker.js
const CACHE_NAME = 'v1';
const ASSETS = ['/', '/index.html', '/app.js', '/style.css'];

self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME).then(cache => cache.addAll(ASSETS))
  );
});

self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then(response => {
      return response || fetch(event.request);
    })
  );
});
```

---

## 缓存失效

- 文件名加 hash：`app.a1b2c3.js`
- 入口 HTML 使用 `no-cache`
- Service Worker 版本号更新

---

*Related: [[09-performance/00-Index|性能优化]], [[08-browser/Cache_Mechanism|浏览器缓存]]*
