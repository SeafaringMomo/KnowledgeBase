# DOM 操作

## 选择元素

```javascript
// 单个元素
document.getElementById('id');
document.querySelector('.class');

// 元素集合
document.querySelectorAll('.item');
document.getElementsByTagName('div');
document.getElementsByClassName('item');
```

---

## 修改元素

```javascript
const el = document.querySelector('#title');

// 内容与属性
el.textContent = 'Hello';
el.innerHTML = '<span>Hello</span>';
el.setAttribute('data-id', '123');
el.classList.add('active');
el.classList.toggle('active');

// 样式
el.style.color = 'red';
el.style.setProperty('--primary', '#007bff');
```

---

## 创建与删除

```javascript
const div = document.createElement('div');
div.textContent = 'New';
document.body.appendChild(div);

// 批量插入（减少重排）
const fragment = document.createDocumentFragment();
for (let i = 0; i < 100; i++) {
  const li = document.createElement('li');
  li.textContent = i;
  fragment.appendChild(li);
}
list.appendChild(fragment);

// 删除
el.remove();
```

---

## 事件处理

```javascript
// 添加事件
button.addEventListener('click', handleClick);

// 事件委托
document.querySelector('ul').addEventListener('click', (e) => {
  if (e.target.tagName === 'LI') {
    console.log(e.target.textContent);
  }
});

// 移除事件
button.removeEventListener('click', handleClick);
```

---

*Related: [[08-browser/00-Index|浏览器]], [[03-javascript/JS_Basics|JavaScript 基础]]*
