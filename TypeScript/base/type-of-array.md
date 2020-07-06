# 数组的类型

在 TypeScript 中，数组类型的定义有多种方法，比较灵活

## [类型 + 方括号] 表示法

最简单的方法就是 [类型 + 方括号] 来表示数组

* 数组的项中不允许出现其他的类型
* 数组的一些方法的参数也会根据数组在定义时约定的类型进行限制
```typescript
let array1: number[] = [1, 1, 2, 3, 5];
let array2: number[] = [1, '1', 2, 3, 5]; //error
array1.push('8') // error 字符串字面量类型
```

## 数组泛型 Array Generic

我们可以使用数组泛型来表示数组 `Array<elemType>`

```typescript
let array: Array<number> = [1, 1, 2, 3, 5]; 
```

## 用接口表示数组

接口也可以用来表述数组

虽然可以，但是我们一般不会这么做，因为这种方式比前两种麻烦多了

```typescript
interface NumberArray {
    [index: number]: number;
}

let number: NumberArray = [1, 1, 2, 3, 5]
```

不过有一种情况除外，那就是用它表示类数组

## 类数组

类数组 Array like Object 不是数组类型，比如 arguments：

```typescript
// error
function sum() {
    let args: number[] = arguments;
}

// true
function sum() {
    let args: {
        [index: number]: number,
        length: number;
        callee: Function;
    } = arguments;
}
```

上面的例子中，除了约束了当前索引的类型是数字时，值的类型必须是数字之外，也约束了它还有 length 和 callee 两个属性

事实上，常用的类数组都有自己的接口定义，如：IArgument、NodeList、HTMLCollection 等：

```typescript
function sum() {
  let args: IArguments = arguments;
}
```

这就是 内置对象

## any 在数组中的应用

一个比较常见的做法是，用 any 表述数组中允许出现任意类型：

```typescript
let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```
