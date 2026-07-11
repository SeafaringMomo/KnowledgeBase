# Grid 布局

## 基础概念

```
┌─────────────────────────────────────────┐
│  ├─ 1fr ─┤├──── 2fr ────┤├── 1fr ──┤   │
│  ┌───────┼───────────┼─────────┐       │
│  │ 1,1   │    1,2    │   1,3   │       │
│  ├───────┼───────────┼─────────┤       │
│  │ 2,1   │    2,2    │   2,3   │       │
│  ├───────┼───────────┼─────────┤       │
│  │ 3,1   │    3,2    │   3,3   │       │
│  └───────┴───────────┴─────────┘       │
│  └────── 3 columns (3列轨道) ────────  │
└─────────────────────────────────────────┘
```

---

## 容器属性

```css
display: grid;
display: inline-grid;

/* 定义列 */
grid-template-columns: 100px 1fr 100px;
grid-template-columns: repeat(3, 1fr);
grid-template-columns: repeat(3, minmax(200px, 1fr));
grid-template-columns: [line1] 100px [line2] 1fr [line3] 100px;

/* 定义行 */
grid-template-rows: 100px auto 200px;
grid-template-rows: repeat(3, minmax(100px, auto));

/* 简写 */
grid-template: 100px 1fr / 200px 1fr 200px;

/* 间隙 */
grid-gap: 20px;          /*行列间隙相同*/
grid-row-gap: 20px;      /*行间隙*/
grid-column-gap: 30px;   /*列间隙*/
grid-gap: 20px 30px;     /*行 20px，列 30px*/

/* 区域模板 */
grid-template-areas:
  "header header header"
  "sidebar main main"
  "footer footer footer";

/* 隐式网格（自动创建） */
grid-auto-rows: 100px;   /* 新行高度 */
grid-auto-columns: 100px;
grid-auto-flow: row;      /* 默认：行优先填入 */
grid-auto-flow: column;   /* 列优先 */
grid-auto-flow: dense;    /* 尽量填充空白 */

/* 简写（推荐） */
grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
```

---

## 项目属性

```css
/* 起始/结束位置 */
grid-column-start: 1;
grid-column-end: 3;
grid-row-start: 1;
grid-row-end: 3;

/* 简写 */
grid-column: 1 / 3;     /* 起始线 / 结束线 */
grid-column: 1 / span 2; /* 起始线 / 跨越2列 */
grid-row: 1 / span 2;

/* 区域命名 */
grid-area: header;       /* 使用模板区域 */

/* 单独对齐 */
align-self: start;       /* 垂直对齐 */
justify-self: start;     /* 水平对齐 */
place-self: center;      /* 两者简写 */

/* 排列顺序 */
order: 0;  /* 默认，按 HTML 顺序 */
```

---

## fr 单位

```css
/* fr = fraction（份） */
grid-template-columns: 1fr 2fr 1fr;
/* 总共4份：各占 1/4, 2/4, 1/4 */
```

---

## minmax() 函数

```css
/* 最小200px，最大1份 */
minmax(200px, 1fr)

/* 响应式列，不换行 */
grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
```

---

## 经典布局

### 圣杯布局
```css
.container {
  display: grid;
  grid-template: auto 1fr auto / 200px 1fr 200px;
  grid-template-areas:
    "header header header"
    "sidebar main aside"
    "footer footer footer";
  height: 100vh;
}
.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }
.aside { grid-area: aside; }
.footer { grid-area: footer; }
```

### 卡片网格（响应式）
```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
}
```

### 某个元素占多列
```css
.featured {
  grid-column: span 2;
  grid-row: span 2;
}
```

---

## 对齐

### 整个网格对齐
```css
justify-content: start;   /* 水平 */
align-content: start;      /* 垂直 */

/* 常用值：start | end | center | space-between | space-around | space-evenly | stretch */
```

### 单元格内对齐
```css
justify-items: stretch;  /* 水平 */
align-items: stretch;    /* 垂直 */

/* 常用值：start | end | center | stretch */
```

---

## 浏览器支持

- IE 10+（仅支持旧语法，需要前缀）
- 现代浏览器完全支持

---

*Related: [[02-css/00-Index|CSS]], [[02-css/Flexbox|Flexbox]]*