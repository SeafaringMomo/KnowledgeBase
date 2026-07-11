---
title: 前端工程化章节索引
tags: [index, 前端, 工程化]
updated: "2026-06-19"
---

# 工程化知识体系

## 📋 目录

- [[06-engineering/Build_Tools|构建工具]] — Webpack、Vite、esbuild
- [[06-engineering/Package_Managers|包管理器]] — npm、pnpm、yarn、镜像源
- [[06-engineering/Code_Standards|代码规范]] — ESLint、Prettier、Husky
- [[06-engineering/CI_CD|CI/CD]] — 自动化构建部署
- [[06-engineering/Modularization|模块化]] — ES Module、CommonJS、Tree Shaking

---

## 主流构建工具对比

| 工具 | 特点 | 适用场景 |
|------|------|----------|
| **Webpack** | 功能全面，生态丰富 | 大型复杂项目 |
| **Vite** | 基于 ESM，启动快 | 现代框架（Vue/React） |
| **esbuild** | Go 编写，极快 | 需要极致性能 |
| **Rollup** | 最佳化输出 | 库、SDK |
| **Parcel** | 零配置 | 小型项目 |

---

## Vite vs Webpack

| 方面 | Vite | Webpack |
|------|------|---------|
| 开发服务器 | ESM 按需编译 | 整包编译 |
| 热更新 | 极快（模块级别） | 较慢（整包重编译） |
| 生产构建 | Rollup | Terser/Webpack |
| 配置 | 极简 | 复杂 |
| 生态 | 增长中 | 成熟 |

---

## package.json 核心字段

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "lint": "eslint .",
    "test": "vitest"
  },
  "dependencies": {
    "react": "^18.2.0",
    "vue": "^3.4.0"
  },
  "devDependencies": {
    "vite": "^5.0.0",
    "typescript": "^5.3.0"
  },
  "engines": {
    "node": ">=18.0.0"
  },
  "sideEffects": false
}
```

---

## 代码规范

### ESLint

```json
{
  "extends": ["eslint:recommended"],
  "parserOptions": {
    "ecmaVersion": 2022,
    "sourceType": "module"
  },
  "rules": {
    "no-unused-vars": "error",
    "no-console": "warn"
  }
}
```

### Prettier

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5",
  "printWidth": 100,
  "tabWidth": 2,
  "arrowParens": "avoid"
}
```

### Git Hooks (Husky)

```bash
npx husky init
npx husky add .husky/pre-commit "npm run lint"
npx husky add .husky/pre-push "npm run test"
```

---

## 模块化

### ES Module

```javascript
// 导出
export const name = "Alice";
export default function foo() {}

// 导入
import { name } from "./module.js";
import foo from "./module.js";
import * as module from "./module.js";

// 动态导入
const module = await import("./module.js");
```

### CommonJS（Node.js）

```javascript
// 导出
module.exports = { name: "Alice" };
exports.name = "Bob";

// 导入
const { name } = require("./module");
```

---

## Git 工作流

```
main ────────────────────────────────────
         ↑                    ↑
     develop                hotfix
         ↓
     feature/login ─────────┘
```

- **Git Flow**: main + develop + feature
- **GitHub Flow**: main + feature（简单）
- **Trunk-Based**: main + 短期分支

---

## CI/CD 示例

```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist
```

---

*Last updated: 2026-06-19*

*Related: [[00-Index|前端知识体系]]*
