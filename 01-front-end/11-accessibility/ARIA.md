# ARIA

## 什么时候使用 ARIA

> 优先使用原生 HTML 语义化元素，ARIA 用于补充无法通过原生标签表达的信息。

---

## 常用角色

```html
<div role="button" tabindex="0">按钮</div>
<div role="dialog" aria-modal="true">弹窗</div>
<nav aria-label="主导航">...</nav>
<div role="alert">错误信息</div>
```

---

## 状态与属性

```html
<button aria-expanded="false" aria-controls="menu">菜单</button>
<div id="menu" hidden>...</div>

<input aria-invalid="true" aria-describedby="error">
<span id="error">输入格式错误</span>

<button aria-pressed="false">收藏</button>
```

---

## ARIA 规则

- 不要修改原生语义（如给 `<button>` 加 `role="link"`）
- 所有交互控件必须可聚焦
- 状态变化需通过程序通知屏幕阅读器

---

*Related: [[11-accessibility/00-Index|可访问性]], [[01-html/Semantic_Tags|语义化标签]]*
