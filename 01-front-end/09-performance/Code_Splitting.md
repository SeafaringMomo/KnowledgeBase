# 代码分割

## 为什么需要代码分割

- 减少首屏加载资源大小
- 按需加载功能模块
- 提高缓存命中率

---

## 动态导入

```javascript
// 普通导入（打包到主包）
import Heavy from './Heavy';

// 动态导入（拆分为独立 chunk）
const Heavy = React.lazy(() => import('./Heavy'));

// 条件加载
if (condition) {
  const module = await import('./feature.js');
  module.run();
}
```

---

## 路由级分割

```jsx
import { createBrowserRouter } from 'react-router-dom';

const router = createBrowserRouter([
  { path: '/', element: <Home /> },
  { path: '/about', element: <About /> }, // React.lazy 包裹
]);
```

```vue
<!-- Vue Router -->
const routes = [
  { path: '/about', component: () => import('./About.vue') }
];
```

---

## 分割策略

| 策略 | 说明 |
|------|------|
| 路由分割 | 按页面拆分 |
| 组件分割 | 大型组件按需加载 |
| 库分割 | 将第三方库单独打包 |
| Worker 分割 | 大型计算放 Web Worker |

---

*Related: [[09-performance/00-Index|性能优化]], [[06-engineering/Build_Tools|构建工具]]*
