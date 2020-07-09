# 枚举

枚举 Enum 类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等

## 简单的例子

枚举使用 enum 关键字来定义

```typescript
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat}
```

枚举成员会被赋值为从 0 开始递增的数字，同时也会对枚举值到枚举名进行反向映射

```typescript
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat}


console.log(Days["Sun"] === 0) // true
console.log(Days["Mon"] === 1) // true
console.log(Days["Tue"] === 2) // true
console.log(Days["Sat"] === 6) // true

console.log(Days[0] === "Sun") // true
console.log(Days[1] === "Mon") // true
console.log(Days[2] === "Tue") // true
console.log(Days[6] === "Sat") // true
```

事实上，上面的例子会被编译为：

```typescript
var Days;
(function(Days) {
    Days[Days["Sun"] = 0] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
```

## 手动赋值

我们也可以给枚举手动赋值：

```typescript
enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true
```

注意，上面未手动赋值的枚举项会接着上一个枚举项递增

并且如果未手动赋值的枚举项与手动赋值的重复了，TypeScript 是不会察觉到这一点的

```typescript
enum Days {Sun = 3, Mon = 1, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 3); // true
console.log(Days["Wed"] === 3); // true
console.log(Days[3] === "Sun"); // false
console.log(Days[3] === "Wed"); // true
```

上面的例子中，递增到 3 的时候与前面的 sun 的取值重复了，但是 TypeScript 并没有报错，导致 Days[3] 的值先是 "Sun" 而后又被 "Wed" 覆盖了

所以使用的时候需要注意，最好不要出现这种覆盖的情况

手动赋值的枚举项可以不是数字，此时需要使用类型断言来让 tsc 无视类型检查（编译出的 js 仍然是可用的）

```typescript
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>"S"};
```

当然，手动赋值的枚举也可以为小数或负数，此时后续未手动赋值的项的递增增长步长仍为 1

```typescript
enum Days {Sun = 7, Mon = 1.5, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1.5); // true
console.log(Days["Tue"] === 2.5); // true
console.log(Days["Sat"] === 6.5); // true
```

## 常数项和计算所得项

枚举项有两种类型：常数项 constant member 和计算所得项 computed member

前面我们所举的例子都是常数项，一个典型的计算所得项的例子：

```typescript
enum Color {Red, Green, Blue = 'blue'.length};
```

上面的例子中，`"blue".length` 就是一个计算所得项

上面的例子不会报错，但是如果紧跟在计算所得项后面的是未手动赋值的项，那么它就会因为无法获得初始值而报错

```typescript
enum Color {Red = "red".length, Green, Blue}; // error
```

## 常数枚举

常数枚举是使用 const enum 定义的枚举类型：

```typescript
const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let disections = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

常数枚举与普通枚举的区别是，它会在编译阶段被删除，并且不能包含计算成员

上例的编译结果是：

```typescript
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

加入包含了计算成员，则会在编译阶段报错：

```typescript
const enum Color {Red, Green, Blue = "blue".length};

// index.ts(1,38): error TS2474: In 'const' enum declarations member initializer must be constant expression.
```

## 外部枚举

外部枚举 Ambient Enums 是使用 declare enum 定义的枚举类型：

```typescript
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

declare 定义的类型只会用于编译时的检查，编译结果中会被删除

上例的编译结果为

```typescript
var directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

当然同时使用 declare 和 const 也是可以的

```typescript
declare const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

编译结果

```typescript
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```
