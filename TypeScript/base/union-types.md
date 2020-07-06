# 联合类型

联合类型表示取值可以是多种类型中的一种

## 简单示例

联合类型使用 `|` 来分割每个类型

```typescript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

## 联合类型的属性和方法

当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法
（也就是这个类型还没有被赋值的时候）

```typescript
function getLength(something: string | number): number {
  return something.length; // error
}

function getString(something: string | number): string {
  return something.toString(); // toString() 是 string 和 number 的共同方法，可以使用
}
```

当 TypeScript 被赋值了之后，会根据类型推论的规则推出一个类型：

```typescript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
console.log(myFavoriteNumber.length);
myFavoriteNumber = 7;
console.log(myFavoriteNumber.length); // error 报错， 识别为 number
```

