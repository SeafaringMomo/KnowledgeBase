# 键盘导航

## 基础原则

所有功能都应能通过键盘完成：

- `Tab`：在可聚焦元素间前进
- `Shift + Tab`：后退
- `Enter` / `Space`：激活按钮或链接
- `Esc`：关闭弹窗/菜单

---

## Tabindex

```html
<!-- 默认可聚焦 -->
<button>默认聚焦</button>

<!-- 显式可聚焦 -->
<div tabindex="0">可通过 Tab 聚焦</div>

<!-- 不可通过 Tab 聚焦，但可通过 JS focus() 聚焦 -->
<div tabindex="-1">仅脚本聚焦</div>

<!-- 不推荐：改变 Tab 顺序 -->
<div tabindex="5">破坏自然顺序</div>
```

---

## 跳过链接

```html
<a href="#main" class="skip-link">跳到主要内容</a>
...
<main id="main">...</main>
```

```css
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #007bff;
  color: white;
  padding: 8px 16px;
  z-index: 100;
}
.skip-link:focus { top: 0; }
```

---

## 菜单键盘交互

```javascript
document.addEventListener('keydown', (e) => {
  if (e.key === 'ArrowDown') {
    // 移到下一项
  } else if (e.key === 'ArrowUp') {
    // 移到上一项
  } else if (e.key === 'Escape') {
    // 关闭菜单
  }
});
```

---

*Related: [[11-accessibility/00-Index|可访问性]], [[11-accessibility/Focus_Management|焦点管理]]*
