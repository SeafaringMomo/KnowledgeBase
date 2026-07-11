# 语义化标签

## 为什么需要语义化

1. **可访问性 (A11y)** — 屏幕阅读器更好地解析页面
2. **SEO** — 搜索引擎更容易理解页面结构
3. **可维护性** — 代码更易读，便于团队协作
4. **标准化** — 符合 Web 标准

---

## 语义化元素

### 页面结构

```html
<body>
  <header>  <!-- 头部：Logo、导航、搜索等 -->
    <nav>导航链接</nav>
  </header>
  
  <main>  <!-- 主内容（每个页面唯一） -->
    <article>  <!-- 独立完整内容，如文章 -->
      <header>文章标题区</header>
      <section>章节1</section>
      <section>章节2</section>
      <footer>文章脚注（作者、日期等）</footer>
    </article>
    
    <aside>  <!-- 侧边栏：相关链接、广告等 -->
    </aside>
  </main>
  
  <footer>  <!-- 底部：版权、联系方式 -->
  </footer>
</body>
```

### 详细语义标签

| 标签 | 语义 | 使用场景 |
|------|------|----------|
| `header` | 区块头部 | 页面顶部、文章标题区 |
| `footer` | 区块底部 | 页面底部、文章脚注 |
| `nav` | 导航 | 主导航、面包屑、页脚链接 |
| `main` | 主内容 | 页面核心内容（唯一） |
| `article` | 文章 | 博客文章、新闻、帖子 |
| `section` | 章节 | 文档中的节/段 |
| `aside` | 侧边栏 | 相关内容、侧边广告 |
| `figure` | 插图 | 图片+图注 |
| `figcaption` | 图注 | `figure` 的标题 |
| `time` | 时间 | `<time datetime="2026-06-04">June 4</time>` |
| `mark` | 标记 | 高亮文本 |
| `address` | 地址 | 联系信息 |

---

## 常见错误

### ❌ 错误用法
```html
<!-- 用 div 代替所有元素 -->
<div class="header"></div>
<div class="nav"></div>
<div class="main"></div>
<div class="footer"></div>
```

### ✅ 正确用法
```html
<header></header>
<nav></nav>
<main></main>
<footer></footer>
```

---

## 标题层级

```html
<article>
  <header>
    <h1>文章主标题</h1>  <!-- h1 每 article 只有一个 -->
    <p>副标题</p>
  </header>
  
  <section>
    <h2>章节标题</h2>  <!-- h2-h6 按层级使用 -->
    <section>
      <h3>子章节</h3>
    </section>
  </section>
</article>
```

**规则**：h1-h6 按顺序使用，不要跳级

---

## 列表语义

```html
<!-- 导航列表 -->
<nav>
  <ul>
    <li><a href="/">首页</a></li>
    <li><a href="/about">关于</a></li>
  </ul>
</nav>

<!-- 有序步骤 -->
<ol>
  <li>第一步</li>
  <li>第二步</li>
</ol>

<!-- 定义列表 -->
<dl>
  <dt>HTML</dt>
  <dd>超文本标记语言</dd>
  <dt>CSS</dt>
  <dd>层叠样式表</dd>
</dl>
```

---

## 强调与重要

```html
<!-- 重要性 -->
<strong>重要内容</strong>

<!-- 强调（语气） -->
<em>强调内容</em>

<!-- 高亮 -->
<mark>搜索高亮</mark>

<!-- 标题中的简称 -->
<h1><abbr title="HyperText Markup Language">HTML</abbr> 入门</h1>
```

---

## 检测工具

- [W3C HTML 验证器](https://validator.w3.org/)
- 浏览器 DevTools — Accessibility 面板
- Lighthouse — A11y 审计

## HTML5 其他语义元素

### 详情与摘要

```html
<details>
  <summary>点击查看更多</summary>
  <p>折叠的详细内容。</p>
</details>

<details open>
  <summary>默认展开</summary>
  <p>默认可见内容。</p>
</details>
```

### 图表与图注

```html
<figure>
  <img src="chart.png" alt="销售趋势图">
  <figcaption>图 1：2024 年季度销售数据</figcaption>
</figure>
```

### 进度与计量

```html
<!-- 进度条 -->
<progress value="70" max="100">70%</progress>

<!-- 计量器 -->
<meter value="6" min="0" max="10">6 out of 10</meter>
<meter value="0.8" low="0.3" high="0.7" optimum="0.9">80%</meter>
```

### 时间与标注

```html
<p>发布于 <time datetime="2026-06-17">2026年6月17日</time></p>
<p>搜索关键词 <mark>前端</mark> 已高亮</p>
<ruby>
  漢 <rp>(</rp><rt>hàn</rt><rp>)</rp>
</ruby>
```

### 文字方向与换行

```html
<p>用户名：<bdi>إيان</bdi></p>
<p>长单词：supercalifragilistic<wbr>expialidocious</p>
```
---

*Related: [[01-html/00-Index|HTML]], [[11-accessibility/00-Index|可访问性]]*