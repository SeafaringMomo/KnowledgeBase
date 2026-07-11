---
title: React 章节索引
tags: [index, 前端, React]
updated: "2026-06-19"
---

# React 知识体系

## 📋 目录

- [[01-react/Hooks|Hooks]] — useState、useEffect、useRef、useMemo、useCallback
- [[01-react/Nextjs_Basics|Next.js 基础]] — App Router、SSR/SSG、项目结构

---

## 核心概念

### 虚拟 DOM
- 内存中的 JavaScript 对象表示 UI
- 状态变更时，生成新虚拟 DOM 树
- 与旧虚拟 DOM 对比（Diff 算法），计算最小变更
- 批量更新真实 DOM

### 单向数据流
```
Props → Component → State Change → Re-render → Children
```

### 组件分类
| 类型 | 特点 |
|------|------|
| Class Component | 16.8前主流，需继承 React.Component |
| Function Component | 16.8后主流，配合 Hooks 使用 |
| Pure Component | 仅当 props/state 变化时重渲染 |
| Controlled Component | 表单数据由 React 控制 |
| Uncontrolled Component | 表单数据由 DOM 控制 |

---

## 📚 资源

- [React 官方文档](https://react.dev/)
- [React Beta 文档](https://react.dev/learn)
- [Airbnb React/JSX 规范](https://airbnb.io/javascript/react/)

---

*Last updated: 2026-06-19*

*Related: [[00-Index|前端知识体系]]*