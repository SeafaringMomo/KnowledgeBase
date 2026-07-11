# HTML 基础

---

## 实例解析

![](https://www.runoob.com/wp-content/uploads/2013/06/02A7DD95-22B4-4FB9-B994-DDB5393F7F03.jpg)

- **<!DOCTYPE html>** 声明为 HTML5 文档
- **<html>** 元素是 HTML 页面的根元素
- **<head>** 元素包含了文档的元（meta）数据，如 <meta charset="utf-8"> 定义网页编码格式为 **utf-8**。
- **<title>** 元素描述了文档的标题
- **<body>** 元素包含了可见的页面内容
- **<h1>** 元素定义一个大标题
- **<p>** 元素定义一个段落
---

## 文档结构

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <!-- 内容 -->
</body>
</html>
```

### DOCTYPE 作用
- 告诉浏览器使用哪个 HTML 版本解析文档
- HTML5: `<!DOCTYPE html>` (简洁，向下兼容)

---

## 元素分类

### 1. 块级元素 (Block)
- 独占一行，可设置宽高
- `div`, `p`, `h1-h6`, `ul/ol/li`, `table`, `form`, `header`, `footer`, `section`, `article`

### 2. 行内元素 (Inline)
- 与其他元素共处一行
- 不可设置宽高（宽高由内容决定）
- `span`, `a`, `strong`, `em`, `label`, `img`（可设置宽高）

### 3. 行内块元素 (Inline-block)
- 共处一行，可设置宽高
- `button`, `select`, `textarea`, `input`

### 4. HTML5 语义化元素
| 标签 | 含义 |
|------|------|
| `header` | 头部区域 |
| `nav` | 导航区域 |
| `main` | 主内容区（每个页面唯一） |
| `article` | 文章内容 |
| `section` | 章节/区块 |
| `aside` | 侧边栏 |
| `footer` | 底部区域 |

---

## 常见元素

### 文本
```html
<h1>-<h6> 标题
<p> 段落
<span> 行内文本容器
<a href="url" target="_blank"> 链接
<strong> 强调（加粗）
<em> 强调（斜体）
<blockquote> 引用块
<pre> 预格式化文本
<code> 代码片段
```

### 列表
```html
<ul>
  <li>无序列表</li>
</ul>
<ol>
  <li>有序列表</li>
</ol>
<dl>
  <dt>术语</dt>
  <dd>定义</dd>
</dl>
```

### 表格
```html
<table>
  <thead>
    <tr><th>表头</th></tr>
  </thead>
  <tbody>
    <tr><td>数据</td></tr>
  </tbody>
  <tfoot>
    <tr><td>脚注</td></tr>
  </tfoot>
</table>
```

### 表单
```html
<form action="/submit" method="POST">
  <input type="text" name="username" required>
  <input type="email" name="email">
  <input type="password" name="pwd">
  <input type="radio" name="gender" value="male">
  <input type="checkbox" name="hobby" value="coding">
  <select name="city">
    <option value="bj">北京</option>
  </select>
  <textarea name="bio" rows="4"></textarea>
  <button type="submit">提交</button>
</form>
```

### 媒体
```html
<img src="url" alt="描述" loading="lazy">
<audio src="url" controls autoplay loop></audio>
<video src="url" controls poster="封面图"></video>
<canvas id="canvas"></canvas>
<svg>...</svg>
<iframe src="url"></iframe>
```

---

## 属性

### 全局属性
- `id` — 唯一标识
- `class` — 样式类
- `style` — 内联样式
- `title` — 悬浮提示
- `data-*` — 自定义数据
- `hidden` — 隐藏元素
- `contenteditable` — 可编辑
- `draggable` — 可拖拽

### 布尔属性
- `required`, `disabled`, `readonly`, `checked`, `selected`
- 只需写属性名，不需赋值

---

## head 元素

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title>页面标题</title>
  
  <!-- 资源链接 -->
  <link rel="stylesheet" href="style.css">
  <link rel="icon" href="favicon.ico">
  <link rel="manifest" href="manifest.json">
  
  <!-- 预加载 -->
  <link rel="preload" href="font.woff2" as="font">
  
  <!-- DNS 预解析 -->
  <link rel="dns-prefetch" href="//cdn.example.com">
  
  <!-- 脚本（通常放 body 末尾） -->
  <script src="app.js" defer></script>
</head>
```

---

## 链接关系 (rel)

| 值 | 作用 |
|------|------|
| `stylesheet` | 外部样式表 |
| `icon` | 网站图标 |
| `manifest` | Web App Manifest |
| `preload` | 预加载资源 |
| `prefetch` | 预取（下次可能用到） |
| `dns-prefetch` | DNS 预解析 |
| `canonical` | 规范化 URL |

## HTML 文档声明补充

`<!DOCTYPE html>` 声明不区分大小写，以下写法均有效：

```html
<!DOCTYPE html>
<!DOCTYPE HTML>
<!doctype html>
<!Doctype Html>
```

> 浏览器默认编码可能无法正确显示中文，建议在 `<head>` 中显式声明 `UTF-8`：
> ```html
> <meta charset="UTF-8">
> ```

---

## 在 HTML 中使用 CSS

| 方式 | 写法 | 适用场景 |
|------|------|----------|
| 内联样式 | `<p style="color:red">` | 临时、单个元素 |
| 内部样式表 | `<head><style>...</style></head>` | 单页样式 |
| 外部样式表（推荐） | `<link rel="stylesheet" href="style.css">` | 多页共享、可缓存 |

> 已废弃的表现型标签/属性：`bgcolor`、`font`、`<center>` 等，应使用 CSS 替代。

---

## 文本格式化标签

| 标签 | 语义 | 示例 |
|------|------|------|
| `<b>` | 粗体（无语义） | `<b>粗体</b>` |
| `<strong>` | 重要/强调 | `<strong>重要</strong>` |
| `<i>` | 斜体（无语义） | `<i>斜体</i>` |
| `<em>` | 强调语气 | `<em>强调</em>` |
| `<small>` | 小号字 | `<small>小字</small>` |
| `<mark>` | 高亮 | `<mark>关键词</mark>` |
| `<del>` | 删除 | `<del>已删除</del>` |
| `<ins>` | 插入 | `<ins>新增</ins>` |
| `<sub>` | 下标 | `H<sub>2</sub>O` |
| `<sup>` | 上标 | `E = mc<sup>2</sup>` |
| `<code>` | 代码片段 | `<code>console.log()</code>` |
| `<pre>` | 预格式化文本 | `<pre>保留 换行</pre>` |
| `<kbd>` | 键盘输入 | `<kbd>Ctrl</kbd> + <kbd>C</kbd>` |
| `<samp>` | 计算机输出样本 | `<samp>Build success</samp>` |
| `<var>` | 变量 | `<var>x</var>` |
| `<abbr>` | 缩写 | `<abbr title="HyperText Markup Language">HTML</abbr>` |
| `<address>` | 联系信息 | `<address>...</address>` |
| `<bdo>` | 文字方向 | `<bdo dir="rtl">文字</bdo>` |
| `<blockquote>` | 长引用 | `<blockquote>...</blockquote>` |
| `<q>` | 短引用 | `<q>...</q>` |
| `<cite>` | 引用作品 | `<cite>《书名》</cite>` |

---

## 链接属性详解

```html
<!-- target 取值 -->
<a href="..." target="_blank">新标签页</a>
<a href="..." target="_self">当前页（默认）</a>
<a href="..." target="_parent">父框架</a>
<a href="..." target="_top">顶层窗口</a>

<!-- 安全与 SEO -->
<a href="..." target="_blank" rel="noopener noreferrer nofollow">安全外链</a>

<!-- 下载 -->
<a href="file.pdf" download="report.pdf">下载报告</a>

<!-- 锚点 -->
<a href="#section1">跳到第 1 部分</a>
<section id="section1">...</section>
```

### 空链接

| 写法 | 是否跳转 | 说明 |
|------|----------|------|
| `href="#"` | 会（跳到顶部） | 占位，常配合 JS 阻止默认行为 |
| `href="javascript:void(0)"` | 否 | 阻止跳转，兼容旧浏览器 |
| `href=""` | 会（刷新当前页） | 需要刷新时使用 |
| `role="button"` | 否 | 链接表现为按钮，需 JS 处理 |

---

## head 元素补充

```html
<head>
  <!-- 基础链接，设置页面中所有相对链接的基准 URL -->
  <base href="https://example.com/images/" target="_blank">

  <!-- 常用 meta -->
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="页面描述">
  <meta name="keywords" content="HTML, CSS, JavaScript">
  <meta name="author" content="作者名">
  <meta http-equiv="refresh" content="30">
  <meta name="theme-color" content="#000000">

  <!-- 无脚本时的回退内容 -->
  <noscript>请启用 JavaScript 以获得最佳体验。</noscript>
</head>
```

---

## 脚本引入

```html
<!-- 内联脚本 -->
<script>
  console.log('Hello');
</script>

<!-- 外部脚本（推荐放在 </body> 前） -->
<script src="app.js"></script>

<!-- async：异步加载，加载完立即执行，不保证顺序 -->
<script async src="analytics.js"></script>

<!-- defer：延迟到 DOM 解析完成后按顺序执行 -->
<script defer src="app.js"></script>
```

| 属性 | 加载时机 | 执行时机 | 是否阻塞 DOM |
|------|----------|----------|--------------|
| 无 | 立即加载 | 立即执行 | 阻塞解析 |
| `async` | 异步加载 | 加载完立即执行 | 不阻塞加载，执行时阻塞 |
| `defer` | 异步加载 | DOM 解析后、DOMContentLoaded 前 | 不阻塞 |

---

## 框架与嵌入式内容

```html
<!-- iframe -->
<iframe src="page.html" width="100%" height="400" style="border:none;" loading="lazy"></iframe>

<!-- 图片地图 -->
<img src="planets.jpg" alt="行星" usemap="#planetmap">
<map name="planetmap">
  <area shape="rect" coords="0,0,82,126" href="sun.html" alt="太阳">
  <area shape="circle" coords="90,58,3" href="mercury.html" alt="水星">
</map>
```
---

*Related: [[01-html/00-Index|HTML]]*