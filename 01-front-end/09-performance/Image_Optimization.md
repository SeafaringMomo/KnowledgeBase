# 图片优化

## 选择合适的格式

| 格式 | 特点 | 适用场景 |
|------|------|----------|
| JPEG | 有损压缩，体积小 | 照片、复杂图像 |
| PNG | 无损，支持透明 | 图标、截图、透明图 |
| WebP | 体积更小，质量高 | 现代浏览器首选 |
| AVIF | 体积最小 | 支持浏览器可用 |
| SVG | 矢量，无限缩放 | 图标、Logo、插画 |

---

## 响应式图片

```html
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="描述" width="800" height="600">
</picture>

<!-- 不同分辨率 -->
<img srcset="img-400.jpg 400w, img-800.jpg 800w, img-1200.jpg 1200w"
     sizes="(max-width: 600px) 400px, (max-width: 1000px) 800px, 1200px"
     src="img-800.jpg" alt="描述">
```

---

## 避免布局偏移

```css
img {
  width: 100%;
  height: auto;
  aspect-ratio: 16 / 9;
}
```

---

*Related: [[09-performance/00-Index|性能优化]], [[09-performance/Lazy_Loading|懒加载]]*
