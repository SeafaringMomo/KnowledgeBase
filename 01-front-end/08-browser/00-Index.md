---
title: 浏览器章节索引
tags: [index, 前端, 浏览器]
updated: "2026-06-19"
---

# 浏览器知识体系

## 📋 目录

- [[08-browser/Rendering_Process|渲染流程]] — 从 HTML 到像素
- [[08-browser/Event_Loop|事件循环]] — 宏任务、微任务
- [[08-browser/DOM_Manipulation|DOM 操作]] — 选择、修改、事件委托
- [[08-browser/Cache_Mechanism|缓存机制]] — 强缓存、协商缓存
- [[08-browser/Storage|存储]] — Cookie、LocalStorage、SessionStorage、IndexedDB

---

## 渲染流程

```
HTML → Tokenizer → Token → Parser (DOM) → DOM Tree
  ↓
CSS → Tokenizer → Token → Parser (CSSOM) → CSSOM Tree
  ↓
DOM + CSSOM → Layout Tree → Paint → Composite
```

### 具体步骤

1. **请求/响应** — 接收 HTML 响应
2. **解析 HTML** — 浏览器解析 HTML 为 Token，构造 DOM 树
3. **解析 CSS** — 解析 CSS，构造 CSSOM 树
4. **合成 Render Tree** — DOM + CSSOM 合成渲染树
5. **Layout（布局）** — 计算每个节点的几何信息（位置、大小）
6. **Paint（绘制）** — 将节点绘制为多个图层的绘制记录
7. **Composite（合成）** — 将多个图层合成为最终页面

### CSS 加载阻塞

```html
<!-- 阻塞渲染 -->
<link rel="stylesheet" href="style.css">

<!-- 非阻塞（JS 加载完成后才应用） -->
<link rel="preload" href="style.css" as="style" onload="this.rel='stylesheet'">

<!-- JS 阻塞解析（默认） -->
<script src="app.js"></script>

<!-- 异步加载，不阻塞 -->
<script async src="app.js"></script>
<!-- defer 顺序执行，DOMContentLoaded 前执行 -->
<script defer src="app.js"></script>
```

---

## 重排与重绘

### 触发重排（Reflow/Layout）

- 元素尺寸、位置变化
- 添加/删除元素
- 内容变化（文字、图片）
- 浏览器窗口尺寸变化
- 获取布局属性（offsetWidth, scrollTop 等）

### 触发重绘（Repaint）

- 颜色、背景、边框变化
- visibility、opacity 变化（不触发重排）

### 优化建议

```javascript
// ❌ 频繁重排
element.style.width = width + "px";
element.style.height = height + "px";
element.style.margin = margin + "px";

// ✅ 批量修改
element.style.cssText = `width: ${width}px; height: ${height}px; margin: ${margin}px`;

// ✅ 使用 CSS 类
element.classList.add("new-style");

// ✅ 缓存布局属性
const width = element.offsetWidth;  // 读取一次
requestAnimationFrame(() => {
  element.style.width = width * 2 + "px";  // 使用缓存值
});

// ✅ 使用 transform/opacity（只触发合成）
element.style.transform = "translateX(100px)";  // 不触发重排重绘
element.style.opacity = "0.5";  // 只触发重绘
```

---

## 📚 资源

- [浏览器的工作原理](https://web.dev/articles/howbrowserswork)
- [渲染性能](https://web.dev/articles/rendering-performance)

---

*Last updated: 2026-06-19*

*Related: [[00-Index|前端知识体系]]*
