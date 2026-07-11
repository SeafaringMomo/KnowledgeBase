# 核心性能指标

## Core Web Vitals

| 指标 | 全称 | 目标值 | 说明 |
|------|------|--------|------|
| LCP | Largest Contentful Paint | < 2.5s | 最大内容绘制 |
| INP | Interaction to Next Paint | < 200ms | 下一次绘制交互延迟 |
| CLS | Cumulative Layout Shift | < 0.1 | 累计布局偏移 |

## 其他重要指标

| 指标 | 说明 |
|------|------|
| FCP | First Contentful Paint，首次内容绘制 |
| FID | First Input Delay，首次输入延迟（被 INP 替代） |
| TTFB | Time to First Byte，首字节时间 |
| TTI | Time to Interactive，可交互时间 |

---

## 测量工具

- Lighthouse
- Chrome DevTools Performance
- WebPageTest
- PageSpeed Insights
- `web-vitals` 库

```bash
npm install web-vitals
```

```javascript
import { onLCP, onINP, onCLS } from 'web-vitals';

onLCP(console.log);
onINP(console.log);
onCLS(console.log);
```

---

*Related: [[09-performance/00-Index|性能优化]], [[08-browser/Rendering_Process|渲染流程]]*
