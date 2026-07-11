# HTML 速查

## 基本文档结构

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>页面标题</title>
</head>
<body>
  <!-- 可见内容 -->
</body>
</html>
```

---

## 全局属性

| 属性 | 说明 |
|------|------|
| `id` | 唯一标识 |
| `class` | 一个或多个类名 |
| `style` | 内联样式 |
| `title` | 悬停提示 |
| `data-*` | 自定义数据 |
| `hidden` | 隐藏元素 |
| `contenteditable` | 可编辑 |
| `tabindex` | Tab 顺序 |
| `lang` | 语言 |

---

## 常用元素速查

| 元素 | 说明 |
|------|------|
| `<h1>` - `<h6>` | 标题 |
| `<p>` | 段落 |
| `<br>` | 换行 |
| `<hr>` | 水平线 |
| `<a href="">` | 链接 |
| `<img src="" alt="">` | 图片 |
| `<ul>` / `<ol>` / `<li>` | 无序/有序列表 |
| `<dl>` / `<dt>` / `<dd>` | 定义列表 |
| `<table>` / `<tr>` / `<td>` / `<th>` | 表格 |
| `<form>` / `<input>` / `<button>` | 表单 |
| `<div>` | 块级容器 |
| `<span>` | 行内容器 |

---

## 字符实体

| 显示 | 实体名 | 实体编号 |
|------|--------|----------|
| 空格 | `&nbsp;` | `&#160;` |
| `<` | `&lt;` | `&#60;` |
| `>` | `&gt;` | `&#62;` |
| `&` | `&amp;` | `&#38;` |
| `"` | `&quot;` | `&#34;` |
| `'` | `&apos;` | `&#39;` |
| `©` | `&copy;` | `&#169;` |
| `®` | `&reg;` | `&#174;` |
| `™` | `&trade;` | `&#8482;` |
| `€` | `&euro;` | `&#8364;` |
| `¥` | `&yen;` | `&#165;` |
| `°` | `&deg;` | `&#176;` |
| `×` | `&times;` | `&#215;` |
| `÷` | `&divide;` | `&#247;` |

---

## 常用属性参考

| 属性 | 适用元素 | 说明 |
|------|----------|------|
| `href` | `<a>`, `<link>` | 链接目标 |
| `src` | `<img>`, `<script>`, `<iframe>` | 资源地址 |
| `alt` | `<img>` | 替代文本 |
| `type` | `<input>`, `<button>` | 控件类型 |
| `value` | `<input>`, `<option>` | 初始值 |
| `placeholder` | `<input>`, `<textarea>` | 占位提示 |
| `disabled` | 表单元素 | 禁用 |
| `readonly` | 表单元素 | 只读 |
| `required` | 表单元素 | 必填 |
| `checked` | `checkbox/radio` | 默认选中 |
| `selected` | `<option>` | 默认选中 |
| `multiple` | `<select>`, `<input type="file">` | 多选 |
| `target` | `<a>`, `<form>` | 打开目标 |

---

## 性能与 SEO 标签

```html
<!-- 预加载关键资源 -->
<link rel="preload" href="style.css" as="style">
<link rel="preload" href="font.woff2" as="font" crossorigin>

<!-- 预连接 / DNS 预解析 -->
<link rel="preconnect" href="https://cdn.example.com">
<link rel="dns-prefetch" href="https://cdn.example.com">

<!-- 懒加载 -->
<img src="image.jpg" loading="lazy" alt="">
<iframe src="page.html" loading="lazy"></iframe>

<!-- 规范 URL -->
<link rel="canonical" href="https://example.com/page">
```

---

*Related: [[01-html/00-Index|HTML]], [[01-html/HTML_Basics|HTML 基础]]*
