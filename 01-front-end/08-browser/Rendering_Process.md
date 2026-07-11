# 浏览器渲染流程

## 关键渲染路径（Critical Rendering Path）

```
HTML → DOM Tree
        ↑
CSS  → CSSOM Tree
        ↓
Render Tree → Layout → Paint → Composite
```

---

## 具体步骤

1. **解析 HTML**
   - 将字节流转换为 Token
   - 构建 DOM 树
   - 遇到 `<script>` 可能阻塞解析

2. **解析 CSS**
   - 构建 CSSOM 树
   - CSS 加载会阻塞渲染

3. **合成渲染树（Render Tree）**
   - 合并 DOM 与 CSSOM
   - 只包含可见节点（排除 `display: none`）

4. **Layout（布局 / 重排）**
   - 计算每个节点的大小与位置

5. **Paint（绘制 / 重绘）**
   - 将节点绘制为图层

6. **Composite（合成）**
   - 将多个图层合成为最终页面

---

## 阻塞渲染的资源

| 资源 | 是否阻塞解析 | 是否阻塞渲染 |
|------|--------------|--------------|
| `<script>`（无 defer/async） | 是 | 是 |
| `<link rel="stylesheet">` | 否 | 是 |
| `@import` CSS | 否 | 是 |
| 图片/字体 | 否 | 否 |

---

## 优化渲染

```html
<!-- 关键 CSS 内联 -->
<style>/* critical css */</style>

<!-- 非关键 CSS 异步加载 -->
<link rel="preload" href="style.css" as="style" onload="this.rel='stylesheet'">

<!-- 脚本延迟执行 -->
<script src="app.js" defer></script>
```

---

*Related: [[08-browser/00-Index|浏览器]], [[09-performance/Core_Metrics|性能指标]]*
