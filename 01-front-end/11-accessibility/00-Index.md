---
title: 可访问性章节索引
tags: [index, 前端, 可访问性]
updated: "2026-06-19"
---

# 可访问性 (Accessibility)

## 📋 目录

- [[11-accessibility/ARIA|ARIA]] — 角色、状态、属性
- [[11-accessibility/Semantic_HTML|语义化 HTML]] — 使用正确的标签表达结构
- [[11-accessibility/Keyboard_Navigation|键盘导航]] — Tab、快捷键、跳过链接
- [[11-accessibility/Focus_Management|焦点管理]] — 焦点样式、焦点陷阱、焦点恢复
- [[11-accessibility/Screen_Readers|屏幕阅读器]] — 测试与优化

---

## 基础概念

### A11Y

Accessibility（可访问性）的缩写，目标是让**所有人**都能使用产品，包括残障人士。

### 用户群体

- 视力障碍（屏幕阅读器、放大镜）
- 听力障碍
- 运动障碍（只用键盘）
- 认知障碍（简化界面）

---

## 语义化 HTML

```html
<!-- ❌ 无意义 -->
<div onclick="handleClick()">点击</div>
<div class="btn">按钮</div>

<!-- ✅ 语义化 -->
<button type="button">按钮</button>
<nav>导航</nav>
<main>主内容</main>
<aside>侧边栏</aside>
```

---

## ARIA 属性

### 角色 (Roles)

```html
<div role="button">Click me</div>
<div role="img" aria-label="描述图片"></div>
<div role="dialog" aria-modal="true"></div>
<div role="navigation" aria-label="主导航"></div>
```

### 状态属性

```html
<button aria-expanded="false" aria-controls="menu">
  菜单
</button>
<div id="menu" hidden></div>

<input aria-invalid="true" aria-describedby="error-msg">
<span id="error-msg" role="alert">错误信息</span>

<button aria-pressed="false">收藏</button>
```

### 标签关联

```html
<!-- aria-label -->
<button aria-label="关闭">×</button>

<!-- aria-labelledby -->
<h2 id="title">标题</h2>
<section aria-labelledby="title"></section>

<!-- aria-describedby -->
<input aria-describedby="hint">
<span id="hint">请输入邮箱地址</span>
```

---

## 键盘可访问

### 焦点管理

```css
/* 焦点样式 */
:focus {
  outline: 2px solid #007bff;
  outline-offset: 2px;
}

/* 跳过链接 */
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #007bff;
  color: white;
  padding: 8px 16px;
  z-index: 100;
}
.skip-link:focus {
  top: 0;
}
```

```html
<a href="#main-content" class="skip-link">跳到主要内容</a>

<nav>导航</nav>
<main id="main-content">主内容</main>
```

### 快捷键

```html
<!-- 使用 aria-keyword -->
<button aria-keyshortcuts="Alt+S">保存</button>

<!-- 实际实现需要监听 -->
document.addEventListener("keydown", (e) => {
  if (e.altKey && e.key === "s") {
    e.preventDefault();
    document.querySelector("[aria-keyshortcuts*='Alt+S']").click();
  }
});
```

---

## 屏幕阅读器测试

### 常用工具

| 系统 | 工具 |
|------|------|
| Windows | NVDA, JAWS |
| macOS | VoiceOver (⌘+F5) |
| Linux | Orca |
| 浏览器插件 | Accessibility Insights |

### 常用命令

| 系统 | 命令 |
|------|------|
| macOS VoiceOver | ⌘+F5 开启 |
| macOS VoiceOver | Ctrl+Option+→ 下一个元素 |
| macOS VoiceOver | Ctrl+Option+← 上一个元素 |
| NVDA | Insert+↓ 朗读全部 |
| NVDA | Tab 下一个可焦点元素 |

---

## 颜色对比度

### WCAG 标准

| 等级 | 正常文本 | 大文本 |
|------|----------|--------|
| AA | 4.5:1 | 3:1 |
| AAA | 7:1 | 4.5:1 |

### 检查工具

- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [axe DevTools](https://www.deque.com/axe/devtools/)
- Chrome DevTools → Elements → Color Picker

---

## 表单无障碍

```html
<!-- 正确关联 -->
<label for="email">邮箱地址</label>
<input type="email" id="email" name="email">

<!-- 必填标记 -->
<label for="name">
  姓名 <span aria-label="必填">*</span>
</label>

<!-- 错误提示 -->
<label for="phone">电话</label>
<input type="tel" id="phone" aria-invalid="true" aria-describedby="phone-error">
<span id="phone-error" role="alert">请输入有效电话号码</span>

<!-- 分组 -->
<fieldset>
  <legend>支付方式</legend>
  <input type="radio" id="credit" name="payment" value="credit">
  <label for="credit">信用卡</label>
  <input type="radio" id="paypal" name="payment" value="paypal">
  <label for="paypal">PayPal</label>
</fieldset>
```

---

## 图片无障碍

```html
<!-- 有意义的图片 -->
<img src="chart.png" alt="2024年销售趋势图，显示Q1-Q4稳步增长">

<!-- 装饰性图片 -->
<img src="decoration.svg" alt="" role="presentation">

<!-- 复杂图片 -->
<figure>
  <img src="infographic.png" alt="信息图">
  <figcaption>
    图1：描述图表中的关键数据...
  </figcaption>
</figure>
```

---

## 多媒体

```html
<!-- 视频字幕 -->
<video controls>
  <track kind="subtitles" src="subtitles.vtt" srclang="zh" label="中文" default>
</video>

<!-- 音频转录 -->
<audio controls>
  <source src="podcast.mp3">
</audio>
<a href="transcript.html">查看文字稿</a>
```

---

## 测试工具

- Lighthouse (Chrome DevTools)
- axe DevTools (浏览器插件)
- WAVE (浏览器插件)
- Accessibility Insights (浏览器插件)
- NVDA/JAWS (屏幕阅读器)

---

## 检查清单

- [ ] 所有图片有 alt 属性
- [ ] 表单有正确的 label 关联
- [ ] 可以只使用键盘导航
- [ ] 焦点样式明显
- [ ] 颜色对比度符合 WCAG AA
- [ ] 视频有字幕/文字稿
- [ ] ARIA 使用正确
- [ ] 不依赖颜色传达信息
- [ ] 错误信息明确

---

*Last updated: 2026-06-19*

*Related: [[00-Index|前端知识体系]], [[01-html/00-Index|HTML]]*