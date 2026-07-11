# 运行优化

## 减少长任务

长任务会阻塞主线程，导致交互卡顿。

```javascript
// 将大任务拆分为多个小任务
function processChunk(items, index = 0) {
  if (index >= items.length) return;
  // 处理一小批
  const chunk = items.slice(index, index + 100);
  chunk.forEach(handleItem);
  requestIdleCallback(() => processChunk(items, index + 100));
}
```

---

## 防抖与节流

```javascript
// 防抖：停止触发后 N 毫秒执行
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}

// 节流：N 毫秒内最多执行一次
function throttle(fn, interval) {
  let last = 0;
  return (...args) => {
    const now = Date.now();
    if (now - last >= interval) {
      last = now;
      fn.apply(this, args);
    }
  };
}
```

---

## 虚拟列表

只渲染可视区域元素，避免大量 DOM 节点。

```jsx
function VirtualList({ items, itemHeight, height }) {
  const [scrollTop, setScrollTop] = useState(0);
  const visibleCount = Math.ceil(height / itemHeight);
  const start = Math.floor(scrollTop / itemHeight);
  const visible = items.slice(start, start + visibleCount + 1);

  return (
    <div style={{ height, overflow: 'auto' }} onScroll={(e) => setScrollTop(e.target.scrollTop)}>
      <div style={{ height: items.length * itemHeight, position: 'relative' }}>
        <div style={{ transform: `translateY(${start * itemHeight}px)` }}>
          {visible.map(item => <Row key={item.id} data={item} />)}
        </div>
      </div>
    </div>
  );
}
```

---

## Web Worker

```javascript
// main.js
const worker = new Worker('worker.js');
worker.postMessage({ type: 'compute', data: largeArray });
worker.onmessage = (e) => console.log(e.data);

// worker.js
self.onmessage = (e) => {
  const result = heavyComputation(e.data);
  self.postMessage(result);
};
```

---

*Related: [[09-performance/00-Index|性能优化]], [[03-javascript/JS_Basics|JavaScript 基础]]*
