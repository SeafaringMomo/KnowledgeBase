---
title: 性能优化章节索引
tags: [index, 前端, 性能]
updated: "2026-06-19"
---

# 性能优化知识体系

## 📋 目录

- [[09-performance/Core_Metrics|核心指标]] — LCP、FID、CLS、INP
- [[09-performance/Loading_Optimization|加载优化]] — 压缩、CDN、预加载
- [[09-performance/Runtime_Optimization|运行优化]] — 防抖节流、虚拟列表、Web Worker
- [[09-performance/Image_Optimization|图片优化]] — 格式、响应式图片、CLS 优化
- [[09-performance/Lazy_Loading|懒加载]] — 图片、组件、路由懒加载
- [[09-performance/Code_Splitting|代码分割]] — 动态导入、路由分割
- [[09-performance/Cache_Strategy|缓存策略]] — 静态资源、Service Worker

---

## Core Web Vitals

| 指标 | 全称 | 目标 | 说明 |
|------|------|------|------|
| LCP | Largest Contentful Paint | < 2.5s | 最大内容绘制 |
| FID | First Input Delay | < 100ms | 首次输入延迟 |
| CLS | Cumulative Layout Shift | < 0.1 | 累计布局偏移 |
| INP | Interaction to Next Paint | < 200ms | 下一帧交互延迟（2024新） |

### LCP 优化

1. 优化服务器响应时间
2. 使用 CDN
3. 优化关键资源（CSS、字体、图片）
4. 预加载关键资源
5. 优化图片格式（WebP、AVIF）

### CLS 优化

```css
/* 预留图片/视频空间 */
img, video {
  width: 100%;
  height: auto;
  aspect-ratio: 16/9;
}

/* 字体加载 */
@font-face {
  font-display: swap;
  /* 或 */
  font-display: optional;
}

/* 避免动态插入内容 */
```

### INP 优化

1. 减少长任务（Long Tasks）
2. 使用 `requestIdleCallback`
3. 避免大型更新
4. 使用 Web Worker

---

## 加载优化

### 资源压缩

```html
<!-- Gzip/Brotli 压缩（服务器配置） -->

<!-- 图片压缩 -->
<img src="photo.jpg" loading="lazy" alt="">
<picture>
  <source srcset="photo.avif" type="image/avif">
  <source srcset="photo.webp" type="image/webp">
  <img src="photo.jpg" alt="">
</picture>
```

### 代码分割

```javascript
// Webpack/Vite
const LazyComponent = React.lazy(() => import("./HeavyComponent"));

// 路由级分割
const router = createBrowserRouter([
  {
    path: "/",
    element: <Home />
  },
  {
    path: "/about",
    element: React.lazy(() => import("./About"))
  }
]);

// 动态 import
const module = await import("./module.js");
```

### 缓存策略

```
Cache-Control: max-age=31536000, immutable  // 静态资源
Cache-Control: no-cache  // 每次都验证
ETag / Last-Modified  // 协商缓存
```

### 预加载/预取

```html
<!-- 预加载关键资源 -->
<link rel="preload" href="main.js" as="script">
<link rel="preload" href="font.woff2" as="font" crossorigin>

<!-- 预取下次可能用的资源 -->
<link rel="prefetch" href="/next-page">

<!-- 预连接 -->
<link rel="preconnect" href="https://cdn.example.com">
```

### 防抖节流

```javascript
// 防抖（debounce）：最后一次调用后 N 秒执行
function debounce(fn, delay) {
  let timer = null;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}

// 节流（throttle）：每 N 秒最多执行一次
function throttle(fn, interval) {
  let lastTime = 0;
  return function (...args) {
    const now = Date.now();
    if (now - lastTime >= interval) {
      lastTime = now;
      fn.apply(this, args);
    }
  };
}

// 使用
window.addEventListener("resize", debounce(handleResize, 300));
window.addEventListener("scroll", throttle(handleScroll, 100));
```

---

## 运行优化

### 虚拟列表

```jsx
// 只渲染可见区域
const VirtualList = ({ items, itemHeight, visibleCount }) => {
  const [scrollTop, setScrollTop] = useState(0);
  
  const startIndex = Math.floor(scrollTop / itemHeight);
  const endIndex = startIndex + visibleCount;
  const visibleItems = items.slice(startIndex, endIndex + 1);
  
  return (
    <div 
      style={{ height: items.length * itemHeight, position: "relative" }}
      onScroll={e => setScrollTop(e.target.scrollTop)}
    >
      <div style={{ transform: `translateY(${startIndex * itemHeight}px)` }}>
        {visibleItems.map(item => <Item key={item.id} {...item} />)}
      </div>
    </div>
  );
};
```

### Web Worker

```javascript
// main.js
const worker = new Worker("worker.js");
worker.postMessage({ type: "compute", data: largeArray });
worker.onmessage = (e) => {
  console.log("Result:", e.data);
};

// worker.js
self.onmessage = (e) => {
  const result = heavyComputation(e.data);
  self.postMessage(result);
};
```

---

## 工具

- Lighthouse — 性能审计
- Chrome DevTools Performance — 性能分析
- WebPageTest — 在线测试
- PageSpeed Insights — Google 评分

---

*Last updated: 2026-06-19*

*Related: [[00-Index|前端知识体系]]*