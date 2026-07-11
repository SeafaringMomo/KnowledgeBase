# 事件循环 (Event Loop)

## 为什么需要事件循环

JavaScript 是**单线程**语言，同一时间只能执行一个任务。

**问题**：如果一个任务耗时很长（如网络请求），后续任务必须等待。

**解决方案**：异步 + 事件循环

---

## 调用栈 (Call Stack)

```javascript
function multiply(a, b) {
  return a * b;
}

function square(n) {
  return multiply(n, n);
}

console.log(square(4));

// 调用栈顺序：
// 1. console.log(square(4))
// 2. square(4)
// 3. multiply(4, 4)
// 4. return 16
// 5. return 16
// 6. console.log 输出
```

---

## 任务队列 (Task Queue / Macrotask Queue)

```javascript
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

console.log("3");

// 输出：1, 3, 2
// 原因：setTimeout 是宏任务，等同步代码执行完才执行
```

### 宏任务 (Macrotask)

| 任务来源 | 示例 |
|----------|------|
| setTimeout | 定时器 |
| setInterval | 间隔执行 |
| I/O 操作 | 文件读写、网络请求 |
| UI 渲染 | 浏览器渲染 |
| requestAnimationFrame | 动画帧 |

---

## 微任务队列 (Microtask Queue)

```javascript
console.log("1");

Promise.resolve().then(() => {
  console.log("2");
});

console.log("3");

// 输出：1, 3, 2
// Promise.then 是微任务，优先级高于宏任务
```

### 微任务 (Microtask)

| 任务来源 | 示例 |
|----------|------|
| Promise.then/catch/finally | Promise 回调 |
| queueMicrotask | 自定义微任务 |
| MutationObserver | DOM 变化监听 |
| process.nextTick | Node.js |

---

## 执行顺序

```
┌─────────────────────┐
│     Call Stack       │ ← 执行同步代码
│    (同步任务队列)     │
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│   Microtask Queue    │ ← 微任务（Promise）
│  (高优先级，先执行)    │
└──────────┬──────────┘
           ↓（清空后）
┌─────────────────────┐
│    Task Queue        │ ← 宏任务（setTimeout）
│   (事件循环遍历)      │
└─────────────────────┘
```

---

## 完整示例

```javascript
console.log("1");  // 同步

setTimeout(() => console.log("2"), 0);  // 宏任务

Promise.resolve().then(() => console.log("3"));  // 微任务

Promise.resolve().then(() => {
  setTimeout(() => console.log("4"), 0);  // 微任务中的宏任务
});

setTimeout(() => console.log("5"), 0);  // 宏任务

console.log("6");  // 同步

// 顺序：
// 1, 6  ← 同步
// 3      ← 微任务
// 2, 5   ← 宏任务
// 4      ← 微任务中的宏任务
```

---

## async/await

```javascript
async function asyncAwait() {
  console.log("1");
  
  await Promise.resolve().then(() => console.log("2"));
  
  console.log("3");
}

asyncAwait();
console.log("4");

// 输出：1, 4, 2, 3
// 原因：
// 1. async 函数同步部分：输出 1
// 2. await 后面的代码会放入微任务队列
// 3. 同步代码继续执行：输出 4
// 4. 微任务执行：输出 2
// 5. async 函数恢复：输出 3
```

### async 原理

```javascript
// async 函数本质上是返回 Promise 的函数
async function foo() {
  return "result";
}
// 等价于
function foo() {
  return Promise.resolve("result");
}
```

---

## Node.js 中的事件循环

```
   ┌───────────────────────────────┐
   │           timers               │  setTimeout, setInterval
   │  ┌───────────────────────────┐ │
   │  │   pending callbacks       │ │  I/O 回调
   │  │  ┌─────────────────────┐  │ │
   │  │  │    idle, prepare    │  │ │  内部使用
   │  │  │  ┌───────────────┐  │  │ │
   │  │  │  │     poll       │  │  │ │  获取新 I/O 事件
   │  │  │  │  ┌─────────┐  │  │  │ │
   │  │  │  │  │   check   │  │  │ │  setImmediate
   │  │  │  │  └─────────┘  │  │  │ │
   │  │  │  └───────────────┘  │  │ │
   │  │  └─────────────────────┘  │ │
   │  └───────────────────────────┘ │
   └───────────────────────────────┘
```

### process.nextTick vs setImmediate

```javascript
process.nextTick(() => console.log("nextTick"));
setImmediate(() => console.log("setImmediate"));

// 输出：
// nextTick  ← process.nextTick 优先级更高
// setImmediate
```

---

## 常见面试题

### 题目1

```javascript
setTimeout(() => console.log("setTimeout"), 0);
Promise.resolve().then(() => console.log("Promise1"));
Promise.resolve().then(() => {
  Promise.resolve().then(() => console.log("Promise2"));
});
Promise.resolve().then(() => console.log("Promise3"));

// 输出：Promise1, Promise3, Promise2, setTimeout
```

### 题目2

```javascript
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}

async function async2() {
  console.log("async2");
}

console.log("script start");

setTimeout(() => console.log("setTimeout"), 0);

async1();

new Promise(resolve => {
  console.log("Promise1");
  resolve();
}).then(() => console.log("Promise2"));

console.log("script end");

// 输出顺序：
// script start
// async1 start
// async2
// Promise1
// script end
// Promise2
// async1 end
// setTimeout
```

---

*Related: [[03-javascript/00-Index|JavaScript]]*