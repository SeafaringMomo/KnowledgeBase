# 浏览器存储

## Cookie

```javascript
document.cookie = "name=alice; path=/; max-age=3600; Secure; SameSite=Strict";
```

| 特性 | 说明 |
|------|------|
| 大小 | 约 4KB |
| 数量 | 每个域名约 20-50 个 |
| 生命周期 | 可设置过期时间 |
| 作用域 |  domain + path |
| 用途 | 身份认证、追踪、个性化 |

---

## localStorage

长期存储，页面关闭后仍然保留。

```javascript
localStorage.setItem('theme', 'dark');
localStorage.getItem('theme');
localStorage.removeItem('theme');
localStorage.clear();
```

- 容量：约 5-10MB
- 仅存储字符串（对象需 `JSON.stringify`）
- 同步 API，大量读写会阻塞主线程

---

## sessionStorage

会话级存储，页面关闭后清除。

```javascript
sessionStorage.setItem('temp', 'value');
sessionStorage.getItem('temp');
```

- 容量：约 5MB
- 仅在同一标签页共享

---

## IndexedDB

浏览器中的结构化数据库，适合大量数据与复杂查询。

```javascript
const request = indexedDB.open('myDB', 1);

request.onupgradeneeded = (event) => {
  const db = event.target.result;
  db.createObjectStore('users', { keyPath: 'id' });
};

request.onsuccess = (event) => {
  const db = event.target.result;
  const tx = db.transaction('users', 'readwrite');
  const store = tx.objectStore('users');
  store.add({ id: 1, name: 'Alice' });
};
```

---

## 存储对比

| 特性 | Cookie | localStorage | sessionStorage | IndexedDB |
|------|--------|--------------|----------------|-----------|
| 容量 | ~4KB | ~5-10MB | ~5MB | 较大（ hundreds MB） |
| 生命周期 | 可设置 | 持久 | 会话 | 持久 |
| 服务端读取 | 是 | 否 | 否 | 否 |
| 异步 | 否 | 否 | 否 | 是 |
| 结构化数据 | 否 | 否 | 否 | 是 |

---

*Related: [[08-browser/00-Index|浏览器]], [[10-security/XSS|XSS 防护]]*
