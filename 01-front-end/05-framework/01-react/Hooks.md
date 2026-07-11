# React Hooks

## useState

```jsx
// 基础用法
const [count, setCount] = useState(0);

// 函数式更新（依赖上一状态）
setCount(prev => prev + 1);

// 对象 state（需要展开合并）
const [state, setState] = useState({ name: "Alice", age: 25 });
setState(prev => ({ ...prev, age: 30 }));

// 惰性初始化（复杂初始值）
const [data, setData] = useState(() => {
  const saved = localStorage.getItem("data");
  return saved ? JSON.parse(saved) : initialValue;
});
```

---

## useEffect

```jsx
useEffect(() => {
  // 副作用逻辑
  
  return () => {
    // 清理逻辑
  };
}, [dependencies]);  // 依赖数组

// 不传依赖：每次渲染后执行
useEffect(() => {});

// 空依赖：仅首次渲染执行
useEffect(() => {}, []);

// 返回函数：组件卸载时执行
useEffect(() => {
  const timer = setInterval(() => tick(), 1000);
  return () => clearInterval(timer);
}, []);
```

### 常见场景

```jsx
// 数据获取
useEffect(() => {
  fetch(`/api/users/${userId}`)
    .then(res => res.json())
    .then(data => setUsers(data));
}, [userId]);

// 订阅
useEffect(() => {
  const subscription = subscribe(handleEvent);
  return () => subscription.unsubscribe();
}, []);

// 操作 DOM
useEffect(() => {
  document.title = `${count} 次点击`;
}, [count]);

// 事件监听
useEffect(() => {
  window.addEventListener("resize", handleResize);
  return () => window.removeEventListener("resize", handleResize);
}, []);
```

---

## useRef

```jsx
// 引用 DOM 元素
const inputRef = useRef(null);
<input ref={inputRef} />;
inputRef.current.focus();
inputRef.current.value;

// 保存可变值（不触发重渲染）
const timerRef = useRef(null);
useEffect(() => {
  timerRef.current = setInterval(() => tick(), 1000);
  return () => clearInterval(timerRef.current);
}, []);

// 存储上一状态
const prevCountRef = useRef();
useEffect(() => {
  prevCountRef.current = count;
}, [count]);
```

---

## useMemo

```jsx
// 记忆化计算结果
const memoizedValue = useMemo(() => {
  return computeExpensiveValue(a, b);
}, [a, b]);

// 依赖数组为空时不更新
const list = useMemo(() => 
  items.filter(item => item.active),
[]);
```

---

## useCallback

```jsx
// 记忆化回调函数
const handleClick = useCallback((event) => {
  console.log(event.target);
  doSomething(a, b);
}, [a, b]);

// 与子组件的 React.memo 配合
const Button = React.memo(({ onClick }) => (
  <button onClick={onClick}>点击</button>
));

<Button onClick={handleClick} />
```

---

## useReducer

```jsx
const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    case "reset":
      return { count: 0 };
    default:
      throw new Error();
  }
}

const [state, dispatch] = useReducer(reducer, initialState);

// 使用
dispatch({ type: "increment" });
```

---

## useContext

```jsx
// 创建 Context
const ThemeContext = React.createContext("light");

// Provider
<ThemeContext.Provider value="dark">
  <App />
</ThemeContext.Provider>

// 消费（类组件）
static contextType = ThemeContext;
render() {
  const theme = this.context;
}

// 消费（函数组件）
const theme = useContext(ThemeContext);
```

---

## 自定义 Hooks

```jsx
// useLocalStorage
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const saved = localStorage.getItem(key);
    return saved ? JSON.parse(saved) : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue];
}

// useDebounce
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => clearTimeout(handler);
  }, [value, delay]);

  return debouncedValue;
}

// useFetch
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    setLoading(true);
    fetch(url)
      .then(res => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [url]);

  return { data, loading, error };
}
```

---

## 规则

1. **只在顶层调用** — 不在循环、条件、嵌套函数中调用
2. **只在 React 函数中调用** — 函数组件、自定义 Hooks

---

*Related: [[01-react/00-Index|React]]*