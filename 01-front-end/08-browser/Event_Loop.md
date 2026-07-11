# 浏览器事件循环

## 单线程与异步

JavaScript 在浏览器中是单线程的，事件循环负责协调同步代码、微任务和宏任务的执行顺序。

---

## 任务队列

```
┌─────────────────────┐
│     Call Stack      │  ← 同步代码
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│   Microtask Queue   │  ← Promise.then / queueMicrotask / MutationObserver
└──────────┬──────────┘
           ↓（清空后）
┌─────────────────────┐
│    Macrotask Queue  │  ← setTimeout / setInterval / I/O / UI事件
└─────────────────────┘
```

---

## 执行顺序

1. 执行同步代码
2. 清空所有微任务
3. 取出一个宏任务执行
4. 再次清空所有微任务
5. 循环步骤 3-4

```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
// 输出：1 4 3 2
```

---

## requestAnimationFrame

```javascript
function animate() {
  // 在下次重绘前执行
  requestAnimationFrame(animate);
}
requestAnimationFrame(animate);
```

> `requestAnimationFrame` 在重绘前执行，比 `setTimeout` 更适合动画。

---

*Related: [[08-browser/00-Index|浏览器]], [[03-javascript/Event_Loop|JS 事件循环]]*
