# 懒加载

## 图片懒加载

```html
<!-- 原生懒加载 -->
<img src="placeholder.jpg" data-src="image.jpg" loading="lazy" alt="">

<!-- iframe 懒加载 -->
<iframe src="page.html" loading="lazy"></iframe>
```

---

## Intersection Observer

```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      observer.unobserve(img);
    }
  });
});

document.querySelectorAll('img[data-src]').forEach(img => observer.observe(img));
```

---

## 路由/组件懒加载

```javascript
// React
const About = React.lazy(() => import('./About'));

// Vue
const About = () => import('./About.vue');
```

---

*Related: [[09-performance/00-Index|性能优化]], [[09-performance/Code_Splitting|代码分割]]*
