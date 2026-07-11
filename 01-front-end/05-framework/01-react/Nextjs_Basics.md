# Next.js 基础

## 目录结构

```
my-nextjs-app/
├── app/                # App Router（Next.js 13+ 推荐）
│   ├── layout.js       # 根布局
│   ├── page.js         # 首页
│   ├── loading.js      # 加载 UI
│   ├── error.js        # 错误 UI
│   └── globals.css     # 全局样式
├── public/             # 静态资源
├── src/                # 可选源码目录
├── next.config.js      # Next.js 配置
├── package.json
└── .eslintrc.json
```

> 传统 `pages/` 目录为 Pages Router，Next.js 13 后推荐使用 `app/`。

---

## 手动创建项目

```bash
mkdir my-nextjs-js-app
cd my-nextjs-js-app
yarn init -y
yarn add next react react-dom
yarn add -D eslint eslint-config-next

# 创建目录与文件
mkdir -p src/app public
touch src/app/layout.js src/app/page.js src/app/globals.css
touch next.config.js .eslintrc.json .gitignore
```

### package.json

```json
{
  "name": "my-nextjs-js-app",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "next": "^14.0.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "eslint": "^8.56.0",
    "eslint-config-next": "^14.0.0"
  }
}
```

### 根布局 layout.js

```jsx
import './globals.css';

export const metadata = {
  title: 'Next.js App',
  description: 'A Next.js app',
};

export default function RootLayout({ children }) {
  return (
    <html lang="zh-CN">
      <body>{children}</body>
    </html>
  );
}
```

### 首页 page.js

```jsx
export default function Home() {
  return (
    <main>
      <h1>Welcome to Next.js</h1>
    </main>
  );
}
```

### next.config.js

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
};

module.exports = nextConfig;
```

---

## 启动项目

```bash
yarn dev     # 开发服务器 http://localhost:3000
yarn build   # 生产构建
yarn start   # 启动生产服务器
yarn lint    # 代码检查
```

---

## App Router 核心特性

| 特性 | 说明 |
|------|------|
| 服务端组件（Server Components） | 默认在服务端渲染，可直接访问数据库/文件系统 |
| 客户端组件 | 文件顶部写 `"use client"`，使用 useState/useEffect 等 |
| 基于文件的路由 | `app/blog/page.js` 对应 `/blog` |
| 嵌套布局 | 每个目录可定义 `layout.js`，嵌套生效 |
| 加载状态 | `loading.js` 自动包裹 Suspense |
| 错误处理 | `error.js` 自动包裹 Error Boundary |

---

## 数据获取

```jsx
// 服务端组件中直接获取数据
async function PostList() {
  const posts = await fetch('https://api.example.com/posts', {
    next: { revalidate: 60 }, // ISR：60 秒重新验证
  });
  const data = await posts.json();
  return (
    <ul>
      {data.map(post => <li key={post.id}>{post.title}</li>)}
    </ul>
  );
}
```

---

*Related: [[01-react/00-Index|React]], [[01-react/Hooks|Hooks]]*
