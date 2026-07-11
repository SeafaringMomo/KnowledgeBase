---
title: JavaScript 章节索引
tags: [index, 前端, JavaScript]
updated: "2026-06-19"
---

# JavaScript 知识体系

## 📋 目录

- [[03-javascript/JS_Basics|JS 基础]] — 变量、数据类型、运算符
- [[03-javascript/Event_Loop|事件循环]] — 宏任务、微任务、Promise

---

## 🔑 核心概念

### 事件循环 (Event Loop)

```
┌──────────────────┐
│     Call Stack   │ ← 执行同步代码
└────────┬─────────┘
         ↓
┌──────────────────┐
│   Microtask Queue │ ← Promise.then, async, MutationObserver
│   (高优先级)       │
└────────┬─────────┘
         ↓ (空时才执行)
┌──────────────────┐
│    Task Queue    │ ← setTimeout, setInterval, I/O
│   (低优先级)       │
└──────────────────┘
```

---

## 📚 资源

- [MDN JavaScript](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)
- [JavaScript 教程](https://javascript.info/)

---

*Last updated: 2026-06-19*

*Related: [[00-Index|前端知识体系]]*