# CSS 基础

## 选择器

### 基础选择器

```css
/* 元素选择器 */
p { color: red; }

/* 类选择器 */
.title { font-size: 24px; }

/* ID 选择器 */
#header { background: #fff; }

/* 通配符选择器 */
* { margin: 0; padding: 0; }

/* 属性选择器 */
input[type="text"] { border: 1px solid #ccc; }
a[href^="https"] { color: green; }     /* 以 https 开头 */
a[href$=".pdf"] { color: red; }       /* 以 .pdf 结尾 */
a[href*="google"] { color: blue; }     /* 包含 google */
```

### 组合选择器

```css
/* 后代选择器 */
.parent .child { color: blue; }

/* 子选择器 */
.parent > .child { color: red; }

/* 相邻兄弟 */
.one + .two { color: green; }

/* 通用兄弟 */
.one ~ .two { color: yellow; }

/* 多选择器 */
h1, h2, h3 { font-weight: bold; }
```

### 伪类与伪元素

```css
/* 伪类 */
:hover { color: blue; }           /* 悬浮 */
:focus { outline: none; }        /* 聚焦 */
:active { color: red; }          /* 点击 */
:visited { color: purple; }      /* 已访问 */
:first-child { color: red; }     /* 第一个子元素 */
:last-child { color: blue; }      /* 最后一个子元素 */
:nth-child(2) { color: green; }  /* 第 N 个子元素 */
:nth-child(odd) { }              /* 奇数 */
:nth-child(even) { }             /* 偶数 */
:nth-child(3n+1) { }            /* 3n+1 个 */

/* 伪元素 */
::before { content: "前缀"; }     /* 元素前插入 */
::after { content: "后缀"; }      /* 元素后插入 */
::first-line { color: red; }      /* 第一行 */
::first-letter { font-size: 2em; } /* 第一个字符 */
::selection { background: yellow; } /* 选中内容 */
```

---

## 优先级 (Specificity)

```
!important > 内联样式 > ID > 类/属性/伪类 > 元素/伪元素 > 通配符
```

| 选择器 | 分值 |
|--------|------|
| `!important` | 最高（慎用） |
| 内联样式 `style="..."` | 1000 |
| `#id` | 100 |
| `.class` / `[attr]` / `:pseudo` | 10 |
| `tag` / `::before` | 1 |
| `*` | 0 |

### 优先级计算示例

```css
/* 分值: 1+10+1 = 12 */
ul li:first-child { }  

/* 分值: 100+10 = 110 */
#nav .item:first-child { }

/* 分值: 100+1 = 101 */
#header { }
```

### 优先级规则

1. 相同分值：后面覆盖前面
2. 分值高的一定赢
3. `!important` 最高（但尽量避免）

---

## 盒模型

```css
/* content-box: 默认，宽高 = content */
box-sizing: content-box;

/* border-box: 宽高 = content + padding + border */
box-sizing: border-box;

/* margin 可为负值，padding 不可为负值 */
margin: -10px;  /* 允许 */
padding: -10px; /* 不允许 */
```

### 外边距合并

```css
/* 垂直方向外边距会合并 */
.box1 { margin-bottom: 20px; }
.box2 { margin-top: 30px; }
/* 实际间距 = max(20px, 30px) = 30px */

/* 水平方向不会合并 */
```

---

## display 属性

| 值 | 说明 |
|------|------|
| `block` | 块级元素，独占一行 |
| `inline` | 行内元素，共处一行 |
| `inline-block` | 行内块，可设宽高 |
| `flex` | 弹性盒（见 Flexbox） |
| `grid` | 网格布局（见 Grid） |
| `none` | 不显示（不占位） |
| `table` | 表格布局 |

---

## 常用属性

### 文本

```css
text-align: left/center/right/justify;
text-decoration: none/underline/overline/line-through;
text-transform: uppercase/lowercase/capitalize;
text-indent: 2em;              /* 首行缩进 */
letter-spacing: 2px;           /* 字间距 */
line-height: 1.5;              /* 行高 */
word-break: break-word;        /* 自动换行 */
```

### 字体

```css
font-family: "Helvetica Neue", Arial, sans-serif;
font-size: 16px;
font-weight: 400/normal/bold/lighter;
font-style: normal/italic;
font: 14px/1.5 "Arial", sans-serif;  /* 简写 */

/* Web 字体 */
@font-face {
  font-family: 'MyFont';
  src: url('font.woff2') format('woff2');
  font-weight: normal;
  font-style: normal;
}
```

### 背景

```css
background-color: #ff0000;
background-image: url('bg.jpg');
background-repeat: repeat/no-repeat/repeat-x/repeat-y;
background-position: center/top/bottom/10px 20px;
background-size: cover/contain/100% 100%;
background-attachment: scroll/fixed/local;

/* 简写 */
background: #f5f5f5 url('bg.jpg') no-repeat center center / cover;
```

### 边框

```css
border: 1px solid #ccc;
border-radius: 4px;
border-radius: 50%;        /* 圆形 */
border-radius: 10px 20px;  /* 椭圆角 */
border-radius: 10px 5px 20px 15px;  /* 左上 右上 右下 左下 */

/* 阴影 */
box-shadow: 0 2px 8px rgba(0,0,0,0.1);
box-shadow: 0 5px 15px rgba(0,0,0,0.3), 0 0 30px rgba(0,0,0,0.1);
```

---

## 继承

```css
/* 可继承的属性 */
color, font-family, font-size, font-weight, font-style, 
line-height, text-align, list-style, cursor, visibility, opacity

/* 强制继承 */
inherit;

/* 强制不继承 */
initial;

/* 恢复为浏览器默认值 */
unset;
```

---

## z-index 层叠

```css
/* 只有定位元素（position: relative/absolute/fixed/sticky）有效 */
position: relative;
z-index: 1;     /* 数值越大越在上层 */
z-index: -1;    /* 可为负数 */

/* 层叠上下文 */
position: relative;
z-index: 0;  /* 产生新的层叠上下文 */

/* 常见问题：父子 z-index 嵌套 */
```

---

## overflow

```css
overflow: visible;     /* 溢出可见 */
overflow: hidden;       /* 溢出隐藏 */
overflow: scroll;       /* 始终显示滚动条 */
overflow: auto;         /* 按需显示滚动条 */
overflow-x: hidden;    /* 水平溢出隐藏 */
overflow-y: auto;       /* 垂直溢出滚动 */

/* 清除浮动 */
overflow: hidden;  /* BFC 方式 */
```

---

## 浮动 (Float)

```css
float: left;
float: right;
float: none;

/* 清除浮动 */
.clearfix::after {
  content: '';
  display: block;
  clear: both;
}
```

> ⚠️ 现代布局推荐使用 Flexbox/Grid，浮动已很少使用

## CSS 注释

```css
/* 单行注释 */
p {
  color: red; /* 行内注释 */
}

/*
  多行注释
  用于说明复杂规则
*/
```

---

## CSS 引入方式

| 方式 | 优先级 | 说明 |
|------|--------|------|
| 内联样式 `style=""` | 最高 | 单个元素，难维护 |
| 内部样式表 `<style>` | 中 | 作用于当前页面 |
| 外部样式表 `<link>` | 低 | 推荐，可缓存、复用 |
| 浏览器默认样式 | 最低 | 用户代理样式表 |

> 来源优先级相同的情况下，后声明的覆盖先声明的（层叠）。

---

## id 与 class 选择器补充

```css
#header { background: #fff; }      /* id 选择器，页面中应唯一 */
.center { text-align: center; }    /* class 选择器，可复用 */
p.center { font-weight: bold; }    /* 仅 p.center 生效 */
.center.color { color: red; }      /* 多 class */
```

> 注意：CSS 类名不能以数字开头，否则在某些浏览器中会失效。
---

*Related: [[02-css/00-Index|CSS]]*