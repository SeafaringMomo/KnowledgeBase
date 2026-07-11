# CSS 动画

## transition

```css
/* 基础语法 */
transition: property duration timing-function delay;

/* 示例 */
.box {
  transition: all 0.3s ease 0s;
  transition: transform 0.3s ease-in-out;
  transition: background-color 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

/* 多属性 */
.box {
  transition: 
    transform 0.3s ease,
    background-color 0.5s ease,
    opacity 0.2s linear;
}
```

### timing-function

| 函数 | 效果 |
|------|------|
| `ease` | 快→慢（默认） |
| `linear` | 匀速 |
| `ease-in` | 慢→快 |
| `ease-out` | 快→慢 |
| `ease-in-out` | 慢→快→慢 |
| `cubic-bezier(n,n,n,n)` | 自定义贝塞尔曲线 |

---

## transform

```css
/* 2D 变换 */
transform: translate(x, y);      /* 平移 */
transform: translateX(100px);
transform: translateY(-50%);
transform: scale(n);             /* 缩放 */
transform: scale(2, 1);          /* x, y */
transform: rotate(45deg);         /* 旋转 */
transform: skew(x, y);           /* 倾斜 */

/* 3D 变换 */
transform: translateZ(100px);
transform: rotateX(45deg);       /* 3D 旋转 */
transform: rotateY(45deg);
transform: rotateZ(45deg);
transform: perspective(1000px);  /* 透视 */

/* 组合 */
transform: translate(100px, 100px) rotate(45deg) scale(1.2);
```

### 中心点

```css
transform-origin: center center;   /* 默认 */
transform-origin: top left;
transform-origin: 50% 50%;
transform-origin: 100px 100px;
```

---

## animation

```css
/* 基础语法 */
animation: name duration timing-function delay count direction fill-mode;

/* 示例 */
@keyframes slideIn {
  from {
    transform: translateX(-100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

.box {
  animation: slideIn 0.5s ease forwards;
}

/* 多动画 */
.box {
  animation: 
    fadeIn 0.3s ease forwards,
    slideIn 0.5s ease 0.3s forwards;
}
```

### 详细属性

```css
animation-name: slideIn;
animation-duration: 0.5s;
animation-timing-function: ease;
animation-delay: 0s;
animation-iteration-count: 1;        /* 循环次数 */
animation-iteration-count: infinite;
animation-direction: normal;        /* 正向 */
animation-direction: reverse;       /* 反向 */
animation-direction: alternate;      /* 交替 */
animation-fill-mode: none;           /* 默认 */
animation-fill-mode: forwards;       /* 停在最后一帧 */
animation-fill-mode: backwards;       /* 停在第一帧 */
animation-fill-mode: both;           /* both */
animation-play-state: running;       /* 播放 */
animation-play-state: paused;        /* 暂停 */
```

### 关键帧示例

```css
/* 弹跳效果 */
@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-20px); }
}

/* 加载动画 */
@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

/* 呼吸效果 */
@keyframes breathe {
  0%, 100% { transform: scale(1); opacity: 0.8; }
  50% { transform: scale(1.05); opacity: 1; }
}
```

---

## 硬件加速

```css
/* 触发 GPU 加速 */
.box {
  transform: translateZ(0);       /* 或 translate3d */
  will-change: transform;         /* 提前告知浏览器 */
  backface-visibility: hidden;   /* 防止闪烁 */
}

/* 性能更好的动画属性 */
transform, opacity, filter, clip-path
/* 性能较差的 */
width, height, margin, padding, left, top
```

---

## requestAnimationFrame

```javascript
// 平滑动画
const element = document.querySelector(".box");
let start = null;
const duration = 1000;

function animate(timestamp) {
  if (!start) start = timestamp;
  const progress = timestamp - start;
  
  element.style.transform = `translateX(${progress}px)`;
  
  if (progress < duration) {
    requestAnimationFrame(animate);
  }
}

requestAnimationFrame(animate);

// 配合 CSS transition/animation 使用
element.style.transition = "transform 0.3s";
element.style.transform = "translateX(100px)";
```

---

## 动画库

| 库 | 特点 |
|----|------|
| **Animate.css** | 预设动画类 |
| **GSAP** | 高性能、功能强大 |
| **Framer Motion** | React 专用 |
| **Vue Transition** | Vue 内置 |
| **Motion** | 轻量、声明式 |

---

*Related: [[02-css/00-Index|CSS]]*