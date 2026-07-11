---
title: JavaScript 与 TypeScript
tags: [技术, 前端, 语言]
updated: "2026-06-18"
---

# JavaScript 与 TypeScript

## 介绍
JavaScript 是 Web 开发的通用语言；TypeScript 在其之上加入了静态类型，提升了开发者的生产力和代码可靠性。

## 核心区别
- **类型系统**：可选的静态类型、接口、枚举。
- **编译过程**：TypeScript 编译为目标 ECMAScript 版本的 JavaScript。
- **工具链**：更丰富的 IDE 支持、自动补全、重构功能。
- **运行时**：编译后类型会被剔除，对运行时无影响。

## 迁移策略
1. 逐步将 `.js` 文件改名为 `.ts`。
2. 在 `tsconfig.json` 中启用 `allowJs`，实现 JS 与 TS 混合编写。
3. 使用 `strict` 模式以获得最高安全性。
4. 通过 `@types/*` 渐进式地添加社区类型定义。

## 常见陷阱
- 模块解析不匹配（如 `esModuleInterop` 配置错误）。
- 过度使用 `any` 丧失类型检查优势。
- 为旧浏览器设置不恰当的 `tsconfig` `target`/`module`。

## 最佳实践
- 更倾向于使用 **接口** 而非 **类型别名** 来描述对象结构。
- 使用 `readonly` 标记不可变数据。
- 当类型不确定时，使用 `unknown` 代替 `any`。
- 开启 `noImplicitAny` 与 `strictNullChecks`。

## 参考资料
- TypeScript 官方网站与手册
- Mozilla MDN JavaScript 指南
- DefinitelyTyped（社区类型定义仓库）
