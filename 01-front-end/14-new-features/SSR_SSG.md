# SSR 与 SSG

## SSR（服务端渲染）

页面在服务端渲染为 HTML 后返回给浏览器。

### 优点

- SEO 友好
- 首屏渲染快
- 社交分享预览正常

### 缺点

- 服务器成本
- 架构复杂
- 动态交互需hydration

### Next.js App Router

```jsx
// app/page.js
async function HomePage() {
  const data = await fetch('https://api.example.com/posts');
  const posts = await data.json();
  return (
    <main>
      {posts.map(post => <h2 key={post.id}>{post.title}</h2>)}
    </main>
  );
}
```

---

## SSG（静态站点生成）

构建时生成静态 HTML，部署到 CDN。

```jsx
// Next.js Pages Router
export async function getStaticProps() {
  const posts = await fetchPosts();
  return {
    props: { posts },
    revalidate: 60, // ISR
  };
}
```

---

## ISR（增量静态再生）

```jsx
export const revalidate = 60;
```

首次请求返回缓存页面，60 秒后在后台重新生成。

---

## 选择策略

| 场景 | 方案 |
|------|------|
| 内容型网站、博客 | SSG |
| 电商、SEO 要求高 | SSR / ISR |
| 纯后台管理系统 | CSR |
| 混合场景 | 按需选择渲染模式 |

---

*Related: [[14-new-features/00-Index|新技术]], [[01-react/00-Index|React]]*
