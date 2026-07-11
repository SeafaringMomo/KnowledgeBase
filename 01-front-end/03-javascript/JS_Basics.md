# JavaScript 基础

## 变量

```javascript
// var：函数作用域，可重复声明（有提升）
var name = 'Alice';

// let：块级作用域，不可重复声明
let age = 25;

// const：块级作用域，声明时必须赋值，不可重新赋值
const PI = 3.14159;

// 暂时性死区（TDZ）
console.log(x);  // ReferenceError
let x = 10;
```

---

## 数据类型

### 原始类型 (Primitives)

```javascript
// 7种原始类型
string    // "hello"
number    // 42, 3.14, NaN, Infinity
bigint    // 9007199254740991n
boolean   // true / false
undefined // 未定义
null      // 空值（typeof null === "object"）
symbol    // Symbol("id")
```

### 引用类型 (Objects)

```javascript
object     // { name: "Alice", age: 25 }
array      // [1, 2, 3]
function   // function() {}
date       // new Date()
regexp     // /pattern/
map        // new Map()
set        // new Set()
weakmap    // new WeakMap()
weakset    // new WeakSet()
```

### 类型判断

```javascript
typeof "hello"        // "string"
typeof 42             // "number"
typeof true           // "boolean"
typeof undefined      // "undefined"
typeof null          // "object" ⚠️ 历史bug
typeof {}            // "object"
typeof []            // "object"
typeof function() {} // "function"
Array.isArray([])     // true

Object.prototype.toString.call([])  // "[object Array]"
```

---

## 类型转换

```javascript
// 字符串转换
String(123)      // "123"
(123).toString() // "123"
123 + ""         // "123"

// 数字转换
Number("42")     // 42
Number("42px")   // NaN
parseInt("42px") // 42
parseFloat("3.14") // 3.14
+ "42"            // 42（隐式转换）

// 布尔转换
Boolean(1)       // true
Boolean(0)       // false
Boolean("")      // false
Boolean("abc")   // true
!!value          // 双取反转布尔
Boolean([])      // true（空数组为真）
Boolean({})      // true（空对象为真）
```

---

## 运算符

### 严格相等
```javascript
==   // 宽松相等，会做类型转换
===  // 严格相等，不转换类型（推荐）

"42" == 42   // true
"42" === 42  // false
```

### 空值合并
```javascript
??  // null/undefined 时返回右侧值
??= // 逻辑赋值

a ?? b      // a !== null && a !== undefined ? a : b
x ??= 10    // x ?? (x = 10)
```

### 可选链
```javascript
obj?.prop         // 属性
arr?.[0]          // 数组
func?.()          // 调用
obj?.nested?.prop  // 链式
```

---

## 字符串

```javascript
const str = "Hello, World!";

// 模板字符串
const msg = `Hello, ${name}!`;  // 支持嵌入变量/表达式

// 常用方法
str.length              // 长度
str.charAt(0)           // 获取字符
str.includes("World")   // 是否包含
str.startsWith("Hello") // 开头
str.endsWith("!")       // 结尾
str.indexOf("o")        // 首次出现位置
str.lastIndexOf("o")    // 最后出现位置
str.replace("World", "JS")  // 替换（返回新字符串）
str.split(",")          // 分割为数组
str.toUpperCase()       // 转大写
str.toLowerCase()       // 转小写
str.trim()              // 去空格
str.trimStart()         // 去开头空格
str.trimEnd()           // 去末尾空格
str.slice(0, 5)         // 切片（含头不含尾）
str.substring(0, 5)     // 切片（不支持负数）
str.padStart(10, "0")   // 前置补全
str.repeat(3)           // 重复
```

---

## 数组

```javascript
const arr = [1, 2, 3, 4, 5];

// 常用方法（不修改原数组，返回新数组）
arr.map(fn)              // 映射
arr.filter(fn)           // 过滤
arr.reduce((acc, cur) => acc + cur, 0)  // 汇总
arr.flat()               // 扁平化
arr.flatMap(fn)         // map + flat
arr.slice(start, end)    // 切片
arr.concat(arr2)         // 拼接
arr.join(",")            // 转字符串
arr.includes(3)          // 是否包含
arr.indexOf(3)           // 位置
arr.find(fn)             // 找第一个匹配
arr.findIndex(fn)        // 找第一个匹配索引
arr.every(fn)            // 是否全部满足
arr.some(fn)             // 是否有满足
arr.sort((a, b) => a - b) // 排序
arr.reverse()            // 反转

// 修改原数组
arr.push(6)      // 末尾添加
arr.pop()        // 末尾删除
arr.unshift(0)   // 头部添加
arr.shift()      // 头部删除
arr.splice(i, n, ...items)  // 插入/删除/替换
arr.sort()       // 原地排序

// 遍历
arr.forEach((item, index) => {})
for (const item of arr) {}
```

---

## 对象

```javascript
const obj = { name: "Alice", age: 25 };

// 访问
obj.name         // 点号
obj["name"]      // 方括号

// 添加/修改/删除
obj.gender = "female";
delete obj.age;

// 判断属性
"name" in obj                    // true（包含原型链）
obj.hasOwnProperty("name")        // true（自有属性）

// 获取键/值
Object.keys(obj)     // ["name", "gender"]
Object.values(obj)   // ["Alice", "female"]
Object.entries(obj)   // [["name", "Alice"], ...]

// 合并
Object.assign({}, obj1, obj2)
{ ...obj1, ...obj2 }             // 推荐

// 拷贝
Object.assign({}, obj)            // 浅拷贝
JSON.parse(JSON.stringify(obj))  // 深拷贝（简陋）
structuredClone(obj)              // 深拷贝（现代）
```

---

## 解构赋值

```javascript
// 对象解构
const { name, age } = obj;
const { name: n, age: a } = obj;  // 重命名
const { name = "Unknown" } = obj; // 默认值
const { nested: { deep } } = obj; // 嵌套

// 数组解构
const [a, b, c] = [1, 2, 3];
const [first, ...rest] = arr;     // 剩余

// 函数参数解构
function foo({ name, age }) { }
foo({ name: "Alice", age: 25 });
```

## 变量使用补充

### 标识符大小写敏感

JavaScript 对大小写敏感，以下是完全不同的标识符：

```javascript
const myVariable = 1;
const MyVariable = 2; // 不同变量
const myvariable = 3; // 不同变量
```

### 注释

```javascript
// 单行注释

/*
  多行注释
*/

/**
 * JSDoc 注释
 * @param {number} a
 * @param {number} b
 */
function add(a, b) {
  return a + b;
}
```

### 函数是一等公民

函数可以像变量一样赋值、传递和返回：

```javascript
const sayHello = function(name) {
  return `Hello, ${name}`;
};

function execute(fn, value) {
  return fn(value);
}

execute(sayHello, 'Alice'); // 'Hello, Alice'
```
---

*Related: [[03-javascript/00-Index|JavaScript]]*