# Flexbox 布局

## 容器属性

```css
/* 设为 Flex 容器 */
display: flex;
display: inline-flex;

/* 主轴方向 */
flex-direction: row;           /* 默认：左→右 */
flex-direction: row-reverse;  /* 右→左 */
flex-direction: column;        /* 上→下 */
flex-direction: column-reverse; /* 下→上 */

/* 主轴排列 */
justify-content: flex-start;   /* 默认：起点对齐 */
justify-content: flex-end;     /* 终点对齐 */
justify-content: center;        /* 居中 */
justify-content: space-between; /* 两端对齐，项目间等距 */
justify-content: space-around;  /* 项目两侧间距相等 */
justify-content: space-evenly;  /* 项目间间距完全相等 */

/* 交叉轴排列 */
align-items: stretch;          /* 默认：拉伸填充 */
align-items: flex-start;      /* 起点对齐 */
align-items: flex-end;        /* 终点对齐 */
align-items: center;            /* 居中 */
align-items: baseline;         /* 基线对齐 */

/* 多行排列 */
flex-wrap: nowrap;   /* 默认：不换行 */
flex-wrap: wrap;     /* 换行 */
flex-wrap: wrap-reverse; /* 反向换行 */

align-content: stretch;
align-content: flex-start;
align-content: flex-end;
align-content: center;
align-content: space-between;
align-content: space-around;

/* 简写 */
flex-flow: row wrap;  /* direction + wrap */
```

---

## 项目属性

```css
/* 伸拉比例 */
flex-grow: 0;  /* 默认：不放大 */
flex-grow: 1;  /* 占据剩余空间 */

/* 缩小比例 */
flex-shrink: 1;  /* 默认：可缩小 */
flex-shrink: 0;  /* 不缩小 */

/* 基准宽度 */
flex-basis: auto;  /* 默认：自动 */
flex-basis: 200px;
flex-basis: 50%;   /* 也可以是百分比 */

/* 简写 */
flex: 1;           /* flex: 1 1 0% */
flex: 0 0 200px;   /* 不伸不缩固定200px */

/* 单独对齐（覆盖 align-items） */
align-self: auto;   /* 默认：继承 align-items */
align-self: flex-start;
align-self: center;
align-self: flex-end;

/* 排列顺序 */
order: 0;  /* 默认：按 HTML 顺序 */
order: -1; /* 排到最前面 */
order: 1;  /* 排到最后面 */
```

---

## 经典布局

### 居中布局
```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

### 两端对齐中间自适应
```css
.parent {
  display: flex;
  justify-content: space-between;
}
```

### Sticky Footer（内容不足时页脚贴底）
```css
body {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
main {
  flex: 1;  /* 占据所有剩余空间 */
}
```

### 栅格系统
```css
.grid {
  display: flex;
  flex-wrap: wrap;
}
.col {
  flex: 1;        /* 等宽 */
}
.col-2 { flex: 0 0 50%; }  /* 固定宽度 */
.col-3 { flex: 0 0 33.33%; }
```

### 列表项
```css
.list {
  display: flex;
  flex-direction: column;
}
.list-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

---

## 常见问题

### 1. flex 元素内的文字不换行
```css
flex-basis: 0;  /* 改为0让文字可以压缩换行 */
```

### 2. flex 子元素高度不一致
```css
/* 设置 align-items: stretch（默认） */
```

### 3. flex-shrink 导致内容被压缩
```css
flex-shrink: 0;  /* 不允许缩小 */
min-width: 0;    /* 允许缩小到更小 */
```

---

## 浏览器支持

- IE 11（部分支持，需要 -webkit- 前缀）
- 现代浏览器完全支持

---

*Related: [[02-css/00-Index|CSS]], [[02-css/Grid|Grid]]*