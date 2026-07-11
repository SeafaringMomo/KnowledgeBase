# JavaScript 面试题

## 1. 闭包

### 什么是闭包？

```javascript
function outer() {
  const x = 10;
  return function inner() {
    console.log(x);  // inner 能访问 outer 的变量
  };
}
const fn = outer();
fn();  // 10
```

**闭包**：函数能记住并访问其词法作用域，即使函数在其词法作用域之外执行。

### 应用场景

```javascript
// 1. 数据私有化
function createCounter() {
  let count = 0;
  return {
    increment() { count++; },
    getCount() { return count; }
  };
}

// 2. 函数柯里化
function currying(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn(...args);
    }
    return (...more) => curried(...args, ...more);
  };
}

// 3. 防抖节流（见性能优化笔记）
```

---

## 2. 原型与原型链

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.sayHi = function() {
  console.log(`Hi, I'm ${this.name}`);
};

const p = new Person("Alice");

// 原型链
p.__proto__ === Person.prototype  // true
Person.prototype.__proto__ === Object.prototype  // true
Object.prototype.__proto__ === null  // true
```

### instanceof 原理

```javascript
function myInstanceOf(left, right) {
  let proto = left.__proto__;
  const prototype = right.prototype;
  while (proto !== null) {
    if (proto === prototype) return true;
    proto = proto.__proto__;
  }
  return false;
}
```

---

## 3. 事件循环

```javascript
console.log("1");

setTimeout(() => console.log("2"), 0);

Promise.resolve().then(() => console.log("3"));

console.log("4");

// 输出顺序：1, 4, 3, 2
// 同步 → 微任务 → 宏任务
```

### 微任务 vs 宏任务

| 微任务 | 宏任务 |
|--------|--------|
| Promise.then | setTimeout |
| MutationObserver | setInterval |
| queueMicrotask | I/O |
| process.nextTick | requestAnimationFrame |

---

## 4. 深拷贝

```javascript
// 简单版
function deepClone(obj) {
  if (obj === null || typeof obj !== "object") return obj;
  const clone = Array.isArray(obj) ? [] : {};
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      clone[key] = deepClone(obj[key]);
    }
  }
  return clone;
}

// 完整版（处理 Symbol、循环引用、函数）
function deepClone(obj, map = new WeakMap()) {
  if (obj === null || typeof obj !== "object") return obj;
  if (map.has(obj)) return map.get(obj);
  
  const clone = Array.isArray(obj) ? [] : {};
  map.set(obj, clone);
  
  const symKeys = Object.getOwnPropertySymbols(obj);
  symKeys.forEach(sym => {
    if (typeof obj[sym] === "object") {
      clone[sym] = deepClone(obj[sym], map);
    }
  });
  
  for (const key in obj) {
    if (Object.prototype.hasOwnProperty.call(obj, key)) {
      clone[key] = deepClone(obj[key], map);
    }
  }
  return clone;
}

// 现代方法
structuredClone(obj);  // 浏览器内置
```

---

## 5. 类型转换

```javascript
// 常见转换题
[] == false     // true（[] ToNumber 为 0，false 为 0）
![]             // false（[] 为 truthy，![] 为 false）
[] === false    // false（类型不同）

// 坑
"10" + 5        // "105"（数字转字符串）
"10" - 5        // 5（字符串转数字）

// 拆箱
Number("10")    // 10
Number([])      // 0
Number({})      // NaN

// 隐式转换
1 + {}          // "1[object Object]"
true + false    // 1
```

---

## 6. this 指向

```javascript
const obj = {
  name: "Alice",
  say() {
    console.log(this.name);
  }
};

// 普通函数：this 取决于调用方式
const say = obj.say;
say();  // undefined（非严格模式为 window）

// 箭头函数：继承外层 this
const obj2 = {
  name: "Bob",
  say: () => console.log(this.name)
};
obj2.say();  // window.name（外层 this）

// new 绑定
new obj.say();  // 新对象

// 显式绑定
obj.say.call({ name: "Carol" });  // Carol
```

---

## 7. Promise

```javascript
// 手写 Promise
class MyPromise {
  constructor(fn) {
    this.state = "pending";
    this.value = undefined;
    this.callbacks = [];
    
    const resolve = (value) => {
      if (this.state !== "pending") return;
      this.state = "fulfilled";
      this.value = value;
      this.callbacks.forEach(cb => cb.onFulfilled(value));
    };
    
    const reject = (reason) => {
      if (this.state !== "pending") return;
      this.state = "rejected";
      this.value = reason;
      this.callbacks.forEach(cb => cb.onRejected(reason));
    };
    
    try {
      fn(resolve, reject);
    } catch (e) {
      reject(e);
    }
  }
  
  then(onFulfilled, onRejected) {
    return new MyPromise((resolve, reject) => {
      const handle = (callback, fallback) => {
        try {
          const fn = typeof callback === "function" ? callback : fallback;
          resolve(fn(this.value));
        } catch (e) {
          reject(e);
        }
      };
      
      if (this.state === "fulfilled") {
        handle(onFulfilled, v => v);
      } else if (this.state === "rejected") {
        handle(onRejected, e => { throw e; });
      } else {
        this.callbacks.push({
          onFulfilled: () => handle(onFulfilled, v => v),
          onRejected: () => handle(onRejected, e => { throw e; })
        });
      }
    });
  }
}
```

---

*Related: [[13-interview/00-Index|面试题]]*