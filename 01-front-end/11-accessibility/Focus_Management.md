# 焦点管理

## 可见的焦点样式

```css
/* 不要完全移除 outline */
:focus-visible {
  outline: 2px solid #007bff;
  outline-offset: 2px;
}
```

---

## 焦点陷阱（Focus Trap）

弹窗打开时，焦点应限制在弹窗内。

```javascript
function trapFocus(element) {
  const focusable = element.querySelectorAll(
    'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
  );
  const first = focusable[0];
  const last = focusable[focusable.length - 1];

  element.addEventListener('keydown', (e) => {
    if (e.key !== 'Tab') return;
    if (e.shiftKey && document.activeElement === first) {
      e.preventDefault();
      last.focus();
    } else if (!e.shiftKey && document.activeElement === last) {
      e.preventDefault();
      first.focus();
    }
  });
}
```

---

## 自动聚焦与恢复

```javascript
// 打开弹窗时聚焦到标题
const dialog = document.querySelector('[role="dialog"]');
dialog.showModal();
dialog.querySelector('h2').focus();

// 关闭弹窗时恢复焦点
const previousFocus = document.activeElement;
dialog.close();
previousFocus.focus();
```

---

*Related: [[11-accessibility/00-Index|可访问性]], [[11-accessibility/Keyboard_Navigation|键盘导航]]*
