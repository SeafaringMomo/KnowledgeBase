# HTML 表单

## 基本结构

```html
<form action="/api/submit" method="POST" enctype="multipart/form-data">
  <!-- 表单内容 -->
</form>
```

### 属性

| 属性 | 说明 |
|------|------|
| `action` | 提交地址 |
| `method` | GET / POST / PUT / DELETE |
| `enctype` | 编码类型：`multipart/form-data`（文件上传）、`application/x-www-form-urlencoded` |
| `target` | 提交后在哪里显示响应 |
| `novalidate` | 禁用浏览器原生验证 |

---

## 输入类型 (input type)

```html
<input type="text">        <!-- 文本 -->
<input type="password">    <!-- 密码 -->
<input type="email">       <!-- 邮箱 -->
<input type="number">      <!-- 数字 -->
<input type="tel">        <!-- 电话 -->
<input type="url">        <!-- URL -->
<input type="search">     <!-- 搜索 -->
<input type="date">        <!-- 日期 -->
<input type="time">        <!-- 时间 -->
<input type="datetime-local">  <!-- 本地日期时间 -->
<input type="month">      <!-- 月份 -->
<input type="week">       <!-- 周 -->
<input type="color">      <!-- 颜色 -->
<input type="range">      <!-- 范围滑块 -->
<input type="file">       <!-- 文件 -->
<input type="hidden">      <!-- 隐藏字段 -->
<input type="checkbox">   <!-- 多选 -->
<input type="radio">      <!-- 单选 -->
```

---

## 表单属性

### 布尔属性

```html
<input required>           <!-- 必填 -->
<input disabled>            <!-- 禁用 -->
<input readonly>            <!-- 只读 -->
<input checked>             <!-- 默认选中（checkbox/radio） -->
<input multiple>            <!-- 多选文件 -->
<input autofocus>           <!-- 自动聚焦 -->
```

### 验证属性

```html
<input type="text" 
       minlength="2" 
       maxlength="10"
       pattern="[A-Za-z]+"
       placeholder="请输入">

<input type="number"
       min="1"
       max="100"
       step="1">

<input type="email"
       pattern="[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,}$">
```

---

## 其他表单元素

```html
<!-- 下拉选择 -->
<select name="city">
  <option value="">请选择</option>
  <option value="bj" selected>北京</option>
  <option value="sh">上海</option>
  <option value="gz">广州</option>
</select>

<!-- 多行文本 -->
<textarea name="bio" rows="4" cols="50" placeholder="自我介绍"></textarea>

<!-- 按钮 -->
<button type="submit">提交</button>
<button type="reset">重置</button>
<button type="button">普通按钮</button>

<!-- 绑定标签 -->
<label for="username">用户名：</label>
<input type="text" id="username" name="username">

<!-- 字段集 -->
<fieldset>
  <legend>个人信息</legend>
  <input type="text" name="name">
</fieldset>
```

---

## 表单验证 API

```javascript
const form = document.querySelector('form');

// 检查有效性
form.checkValidity();

// 获取验证消息
input.validity.valid;           // boolean
input.validity.valueMissing;    // 为空
input.validity.typeMismatch;   // 类型不匹配
input.validity.patternMismatch;// 正则不匹配
input.validity.tooLong;         // 超出最大长度
input.validity.tooShort;        // 不足最小长度
input.validity.rangeUnderflow;  // 小于最小值
input.validity.rangeOverflow;  // 大于最大值
input.validity.stepMismatch;    // 步长不匹配

// 自定义验证
input.setCustomValidity('自定义错误信息');

// 报告错误
input.reportValidity();

// 阻止表单提交（自定义验证失败时）
form.addEventListener('submit', (e) => {
  if (!form.checkValidity()) {
    e.preventDefault();
  }
});
```

---

## CSS 样式

```css
/* 输入框基础样式 */
input, select, textarea {
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

/* 聚焦状态 */
input:focus, select:focus, textarea:focus {
  outline: none;
  border-color: #007bff;
  box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.25);
}

/* 验证失败状态 */
input:invalid:not(:placeholder-shown) {
  border-color: #dc3545;
}

/* 禁用状态 */
input:disabled, button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}
```

---

## 无障碍注意事项

```html
<!-- 每个 input 都应该有 label -->
<label for="name">姓名</label>
<input type="text" id="name" name="name">

<!-- aria-describedby 关联错误提示 -->
<input type="email" 
       id="email" 
       aria-describedby="email-hint email-error">
<span id="email-hint">请输入有效的邮箱地址</span>
<span id="email-error" role="alert">邮箱格式不正确</span>

<!-- 必填字段标记 -->
<label for="phone">
  电话 <span aria-label="必填">*</span>
</label>
```

---

## 现代表单特性

```html
<!-- 输入建议列表 -->
<input type="text" list="countries">
<datalist id="countries">
  <option value="中国">
  <option value="美国">
  <option value="日本">
</datalist>

<!-- 密码显示切换 -->
<label for="password">密码</label>
<input type="password" id="password">
<button type="button" onclick="togglePassword()">显示</button>

<script>
function togglePassword() {
  const input = document.getElementById('password');
  input.type = input.type === 'password' ? 'text' : 'password';
}
</script>
```

## 更多表单元素

### 下拉选项分组

```html
<select name="car">
  <optgroup label="德国车">
    <option value="bmw">宝马</option>
    <option value="audi">奥迪</option>
  </optgroup>
  <optgroup label="日本车">
    <option value="toyota">丰田</option>
    <option value="honda">本田</option>
  </optgroup>
</select>
```

### 计算输出

```html
<form oninput="x.value = parseInt(a.value) + parseInt(b.value)">
  <input type="range" id="a" value="50"> +
  <input type="number" id="b" value="50"> =
  <output name="x" for="a b">100</output>
</form>
```

### 文件与其他 HTML5 类型

```html
<input type="file" multiple accept=".jpg,.png">
<input type="color" value="#ff0000">
<input type="range" min="0" max="100" step="5">
<input type="time">
<input type="datetime-local">
```
---

*Related: [[01-html/00-Index|HTML]], [[11-accessibility/00-Index|可访问性]]*