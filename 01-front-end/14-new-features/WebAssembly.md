# WebAssembly

## 概述

WebAssembly（Wasm）是一种低层级、可移植的二进制格式，允许在浏览器中以接近原生的性能运行 C/C++/Rust 等语言编写的代码。

---

## 适用场景

- 图像/视频处理
- 游戏引擎
- 加密计算
- 科学计算
- 音视频编解码

---

## 使用示例

```rust
// lib.rs
#[no_mangle]
pub extern "C" fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

```javascript
const response = await fetch('add.wasm');
const bytes = await response.arrayBuffer();
const { instance } = await WebAssembly.instantiate(bytes);

console.log(instance.exports.add(1, 2)); // 3
```

---

## 与 JavaScript 的关系

- Wasm 不能替代 JS，而是补充性能密集型任务
- 通过 JS 加载和调用 Wasm 模块
- 共享内存（SharedArrayBuffer）可实现高效通信

---

*Related: [[14-new-features/00-Index|新技术]], [[03-javascript/JS_Basics|JavaScript 基础]]*
