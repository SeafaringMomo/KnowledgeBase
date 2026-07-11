# TypeScript 基础

## 基础类型

```typescript
// 原始类型
let name: string = "Alice";
let age: number = 25;
let isActive: boolean = true;
let bigNum: bigint = 100n;
let u: undefined = undefined;
let n: null = null;
let sym: symbol = Symbol("id");

// any（任意类型，禁用）
let anyValue: any = 42;
anyValue = "string";  // 不报错

// unknown（安全版 any）
let unknownValue: unknown = 42;
// 需要类型 narrowing 才能使用
if (typeof unknownValue === "string") {
  console.log(unknownValue.toUpperCase());
}

// never（永不返回）
function throwError(): never {
  throw new Error("Error");
}
function infiniteLoop(): never {
  while (true) {}
}

// void（无返回值）
function log(message: string): void {
  console.log(message);
}
```

---

## 数组

```typescript
// 两种声明方式
let nums: number[] = [1, 2, 3];
let nums2: Array<number> = [1, 2, 3];

// 只读数组
let readonly: ReadonlyArray<number> = [1, 2, 3];
let tuple: readonly [string, number] = ["hello", 42];

// 元组（固定长度固定类型）
let tuple2: [string, number, boolean] = ["a", 1, true];
tuple2[0] = "b";       // OK
// tuple2[3] = "c";    // Error: 越界
tuple2.push("d");     // ⚠️ 越界写入不报错（TS局限）
```

---

## 对象类型

```typescript
// interface
interface User {
  name: string;
  age: number;
  email?: string;         // 可选
  readonly id: number;   // 只读
  [key: string]: any;   // 动态属性
}

// type 别名
type User = {
  name: string;
  age: number;
};

// 嵌套
interface Config {
  db: {
    host: string;
    port: number;
    database: string;
  };
}

// 空对象
type EmptyObject = Record<string, never>;
```

---

## 联合类型与交叉类型

```typescript
// 联合类型（或）
type StrOrNum = string | number;
let value: StrOrNum = "hello";
value = 42;

// 交叉类型（且）
type Name = { name: string };
type Age = { age: number };
type Person = Name & Age;

// 收缩类型
function padLeft(value: string | number, padding: string | number) {
  if (typeof value === "number") {
    return String(padding).repeat(value);  // value 是 number
  }
  return padding + value;                   // value 是 string
}
```

---

## 断言

```typescript
// 类型断言（不改变运行时）
let someValue: any = "hello";
let strLength: number = (someValue as string).length;
let strLength2: number = (<string>someValue).length;  // JSX 中禁用

// 非空断言
let nullable: string | null = "hello";
console.log(nullable!.toUpperCase());  // 确定非空

// 确定赋值断言
let global: string;  // 声明但未初始化
console.log(global!);  // 告诉 TS 会被赋值
```

---

## 函数

```typescript
// 函数声明
function add(a: number, b: number): number {
  return a + b;
}

// 函数表达式
const add = (a: number, b: number): number => a + b;

// 可选参数
function greet(name: string, greeting?: string): string {
  return greeting ? `${greeting}, ${name}!` : `Hello, ${name}!`;
}

// 默认参数
function greet(name: string, greeting: string = "Hello"): string {
  return `${greeting}, ${name}!`;
}

// 剩余参数
function sum(...nums: number[]): number {
  return nums.reduce((a, b) => a + b, 0);
}

// 箭头函数
const sum = (...nums: number[]): number => nums.reduce((a, b) => a + b, 0);

// this 类型
interface UIElement {
  onClick(this: void, event: Event): void;
}

// 重载
function reverse(x: string): string;
function reverse(x: number): number;
function reverse(x: string | number): string | number {
  if (typeof x === "string") {
    return x.split("").reverse().join("");
  }
  return Number(x.toString().split("").reverse().join(""));
}
```

---

## 枚举

```typescript
// 数字枚举
enum Direction {
  Up,    // 0
  Down,  // 1
  Left,  // 2
  Right, // 3
}
enum Direction {
  Up = 1,
  Down,  // 2
  Left,  // 3
  Right, // 4
}

// 字符串枚举
enum Direction {
  Up = "UP",
  Down = "DOWN",
}

// 常量枚举（编译时内联）
const enum HttpStatus {
  OK = 200,
  NotFound = 404,
}

// 异构枚举（混合）
enum BooleanLike {
  No = 0,
  Yes = "YES",
}

// 使用
Direction.Up        // 1
Direction[1]        // "Up"（反向映射，数字枚举）
HttpStatus.OK       // 200
```

---

## keyof 与 typeof

```typescript
// keyof：获取类型的所有键
interface User {
  name: string;
  age: number;
}
type UserKeys = keyof User;  // "name" | "age"

// typeof：获取值的类型
const user = { name: "Alice", age: 25 };
type UserType = typeof user;  // { name: string; age: number }

// 组合使用
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
getProperty(user, "name");  // string
```

---

*Related: [[04-typescript/00-Index|TypeScript]]*