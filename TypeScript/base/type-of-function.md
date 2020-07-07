# 函数的类型

> 函数是 JavaScript 中的一等公民

## 函数声明

在 JavaScript 中， 有两种常见的定义函数的方法：函数声明 Function Declaration 和函数表达式 Function Expression

```javascript
// 函数声明
 function sum(x, y) {
   return x + y;
 }

// 函数表达式
let mySum = function(x, y) {
  return x + y;
}
```

一个函数有输入和输出，要在 TypeScript 中对其进行约束，需要把输入和输出都考虑到，其中函数声明的类型定义比较简单

```typescript
function sum(x: number, y: number): number {
  return x + y;
}
```

注意，输入多于或少于要求的参数，是不被允许的

## 函数表达式

如果我们现在写一个函数表达式的定义，可能会写成这样：

```typescript
let mySum = function(x: number, y: number): number {
  return x + y;
}
```

上面的代码是可以通过编译的，但是事实上，上面的代码只对等号右面的匿名函数进行了类型定义，而左面的 mySum 是通过赋值操作进行类型推论推断出来的
如果我们手动给 mySum 添加类型，则应该是这样的：

```typescript
let mySum: (x: number, y: number) => number = function(x: number, y: number): number {
  return x + y;
}
```

注意，不要混淆 TypeScript 中的 => 和 ES6 中的 => 

在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型

## 用接口定义函数的形状

我们也可以使用接口的方式来定义一个函数需要符合的形状

```typescript
interface SearchFunc {
    (source: string, subSting: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  return source.search(subString) !== -1;
}
```

采用函数表达式 | 接口定义函数的方式时，对等号左侧进行类型限制，可以保证以后对函数名赋值时保证参数个数、参数类型、返回值类型不变

## 可选参数

使用 可选参数 可以定义可选的参数，与接口中的可选参数一样，可以使用 ? 表示可选参数

```typescript
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return  firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

注意，可选参数必须接在必须参数的后面，换句话说，可选参数后面不允许再出现必需参数了

## 参数默认值

在 ES6 中，我们允许给函数的参数添加默认值，TypeScript 会将添加了默认值的参数识别为可选参数：

```typescript
function buildName(firstName: string, lastName: string = 'Cat') {
  return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

而且在此时就不受 可选参数必须在必选参数后面 的限制了

## 剩余参数

ES6 中，可以使用 ...rest 的方式获取函数中的剩余参数（rest参数）：

```typescript
function push(array, ...items) {
    items.forEach(function(item) {
        array.push(item)
    })
}

let a: any[] = [];
push(a, 1, 2, 3);
```

事实上，上面的 items 就是一个数组，可以使用数组的类型来定义它

```typescript
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

注意，rest 参数只能是最后一个参数

## 重载

重载允许一个函数在接受不同数量或类型的参数时，做出不同的处理

利用联合类型我们可以这么实现： 

```typescript
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('')
    }
}
```

然而上面这种情况有一个缺点，就是不能精准的表达，输入数字的时候，输出也应该是数字，输入字符串的时候，输出也应该是字符串。

这时，我们可以使用重载定义多个 reverse 的函数类型：

```typescript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('')
    }            
}
```

上面的例子中，我们重复定义了多次函数 reverse ，前几次都是函数定义，最后一次是函数的实现，在编辑器的代码提示中，可以正确的看到前两个提示

注意，TypeScript 会优先从最前面的函数定义开始匹配，所有多个函数定义如果有包含关系，需要优先把精确的定义写在前面
