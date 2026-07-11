# 加载优化

## 资源压缩

- Gzip / Brotli 压缩
- 图片压缩与格式优化（WebP / AVIF）
- Tree Shaking 移除无用代码
- Minify JS/CSS/HTML

---

## CDN 与 HTTP/2

- 将静态资源部署到 CDN，缩短物理距离
- HTTP/2 多路复用减少连接开销
- HTTP/3 进一步降低延迟

---

## 预加载与预连接

```html
<!-- 预加载关键字体 -->
<link rel="preload" href="font.woff2" as="font" crossorigin>

<!-- 预连接第三方域名 -->
<link rel="preconnect" href="https://cdn.example.com">

<!-- DNS 预解析 -->
<link rel="dns-prefetch" href="https://api.example.com">

<!-- 预取下一页资源 -->
<link rel="prefetch" href="/next-page">
```

---

## 关键渲染路径优化

- 内联关键 CSS
- 异步加载非关键 CSS
- 延迟加载 JavaScript
- 减少重定向与请求链

---

*Related: [[09-performance/00-Index|性能优化]], [[09-performance/Code_Splitting|代码分割]]*
