# 模块化

## 为什么需要模块化

- 避免全局变量污染
- 代码复用与职责分离
- 清晰的依赖关系
- 便于构建工具进行 Tree Shaking

---

## ES Module（ESM）

现代浏览器与 Node.js 推荐的模块化标准。

```js
// math.js
export const add = (a, b) => a + b;
export default function subtract(a, b) {
  return a - b;
}

// app.js
import subtract, { add } from './math.js';
import * as math from './math.js';

// 动态导入（懒加载）
const module = await import('./math.js');
```

---

## CommonJS（CJS）

Node.js 早期模块规范，广泛用于服务端与工具链。

```js
// math.js
const add = (a, b) => a + b;
module.exports = { add };

// app.js
const { add } = require('./math.js');
```

---

## UMD / AMD

| 规范 | 说明 |
|------|------|
| AMD | RequireJS，浏览器端异步加载 |
| UMD | 兼容 AMD、CJS 与全局变量 |
| IIFE | 立即执行函数，早期模块化方案 |

---

## Tree Shaking

构建工具通过静态分析移除未使用的代码：

```js
// utils.js
export const foo = () => {};
export const bar = () => {};

// app.js
import { foo } from './utils.js'; // bar 会被 Tree Shaking 移除
```

> ESM 的静态导入更利于 Tree Shaking；CommonJS 动态 require 较难优化。

---

*Related: [[06-engineering/00-Index|工程化]], [[03-javascript/JS_Basics|JavaScript 基础]]*
