---
title: Vue 章节索引
tags: [index, 前端, Vue]
updated: "2026-06-19"
---

# Vue 知识体系

## 📋 目录

- [[02-vue/Vue_Basics|Vue 基础]] — 模板、响应式、组件

---

## 核心概念

### 响应式原理
- **Vue 2**: `Object.defineProperty`（无法检测数组下标、添加属性）
- **Vue 3**: `Proxy`（完全代理，更完整、更高效的响应式）

### 声明式 vs 命令式
```javascript
// 命令式（jQuery）
const btn = document.querySelector("#btn");
btn.addEventListener("click", () => {
  count++;
  btn.textContent = count;
});

// 声明式（Vue）
// <button @click="count++">{{ count }}</button>
```

### 组件化
```
App
├── Header
├── Main
│   ├── Sidebar
│   └── Content
│       └── Article
└── Footer
```

---

## Vue 2 vs Vue 3

| 特性 | Vue 2 | Vue 3 |
|------|-------|-------|
| 响应式 | Object.defineProperty | Proxy |
| 生命周期 | beforeCreate/created | setup |
| 组件定义 | Options API | Composition API |
| 状态管理 | Vuex | Pinia |
| TypeScript 支持 | 有限 | 完整 |
| 体积 | ~33KB | ~22KB (tree-shaking) |
| 性能 | 良好 | 更好（静态提升、缓存等） |

---

## 📚 资源

- [Vue 官方文档](https://vuejs.org/)
- [Vue 3 文档](https://vuejs.org/guide/)
- [Vue School](https://vueschool.io/)

---

*Last updated: 2026-06-19*

*Related: [[00-Index|前端知识体系]]*