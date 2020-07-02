# 原始数据类型

* JavaScript 包括原始类型和对象类型
* 原始数据类型包括：布尔值、数值、字符串、null、undefined 和 ES6 的 Symbol

## 布尔值 boolean

在 TypeScript 中，boolean 是 JavaScript 中的基本类型，而 Boolean 是 JavaScript 中的构造函数

```typescript
let isDone: boolean = false; // true

// 注意：使用 Boolean 创造的对象不是布尔值，而是一个 Boolean 对象
let createdByNewboolean: boolean = new Boolean(1);  
    // Type 'Boolean' is not assignable to type 'boolean'
    // 'boolean' is a primitive, but 'Boolean' is a wrapper object. Prefer using 'boolean' when possible.

let createdByNewBoolean: Boolean = new Boolean(1); // true

// 但是直接调用 Boolean 也可以返回一个 boolean 类型
let createdByBoolean: boolean = Boolean(1); // true 

```

## 数值 number

在 TypeScript 中二进制和八进制会被编译为十进制数字

```typescript
// 编译前
let deLiteral: number = 6;
let hextLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

```typescript
// 编译后
var deLiteral = 6;
var hextLiteral = 0xf00d;
var binaryLiteral = 10;
var octalLiteral = 484;
var notANumber = NaN;
var infinityNumber = Infinity;
```

## 字符串 string

```typescript
// 编译前
let myName: string = 'Tom';
let myAge: number = 25;

let sentence: string = `Hello, my name is ${myName}. I'll be ${myAge + 1} years old next mouth.`;
```

```typescript
// 编译后
var myName = 'Tom';
var myAge = 25;
var sentence = "Hello, my name is " + myName + ". I'll be " + (myAge + 1) + "years old next mouth.";
```

## 空值 void

在 JavaScript 中没有空值的概念，在 TypeScript 中，可以用 void 表示没有任何返回值的函数

```typescript
function alertName(): void {
  alert("my name is Tom");
}
```

声明一个 void 类型的变量没有什么用，因为你只能将它赋值为 undefined 和 null 

```typescript
let unusable: void = undefined;
```

## Null 和 Undefined

在 TypeScript 中，可以使用 null 和 undefined 来定义这两个原始数据

```typescript
let u: undefined = undefined;
let n: null = null;
```

与 void 的区别是，undefined 和 null 是所有类型的子类型，也就是说 undefined 类型的变量，可以赋值给 number 类型的变量

```typescript
// 以下都不会报错
let num: number = undefined;

let u: undefined;
let str: string = u;
```

而 void 类型的变量不能赋值给 number 类型的变量

```typescript
let u: void;
let num: number = u;
// Type 'void' is not assignable to type 'number'.
```
