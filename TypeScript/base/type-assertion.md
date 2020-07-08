# 类型断言

类型断言 Type Assertion 可以用手动指定一个值的类型

## 语法

```
值 as 类型

或者

<类型>值
```

注意，在 tsx 中，只能使用 前者

形如 <Foo> 的语法在 tsx 中表示的是一个 ReactNode，在 ts 中除了表示类型断言之外，也可能是表示一个泛型

所以建议只使用 `值 as 类型` 这一种语法

## 类型断言的用途

类型断言的常见用途有以下几种：

### 将一个联合类型断言为其中一个类型

之前有提过，当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型中共有的属性或方法

但是有些时候，我们确实需要在不确定类型的时候就访问其中一个类型特有的属性或方法，比如：

```typescript
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function isFish(animal: Cat | Fish) {
    if (typeof animal.swim === 'function') { // error
        return true;
    }
    return false;
}
```

在上面的例子中，获取 animal.swim 的时候会报错

此时可以使用类型断言，将 animal 断言成 Fish：

```typescript
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function isFish(animal: Cat | Fish) {
    if (typeof (animal as Fish).swim === 'function') {
        return true;
    }
    return false;
}
```

**需要注意的是，类型断言只能 欺骗 TypeScript 编译器，无法避免运行时的错误，反而滥用类型断言可能会导致运行时错误**

```typescript
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function swim(animal: Cat | Fish) {
    (animal as Fish).swim();
}

const tom: Cat = {
    name: 'Tom',
    run() { console.log('run') }
}

swim(tom);
// Uncaught TypeError: animal.swim is not a function
```

上面的例子编译时不会报错，但在运行时会报错

原因是 `(animal as Fish).swim()` 这段代码隐藏了 animal 可能为 Cat 的情况，将 animal 直接断言为 Fish 了，
而 TypeScript 编译器信任了我们的断言，故在调用 swim() 时没有编译错误

可是 swim 函数接收的参数是 Cat | Fish 一旦传入的参数是 Cat 类型的变量， 由于 Cat 上没有 swim 方法，就会导致运行时错误了。

### 将一个父类断言为更加具体的子类

当类之间有继承关系的时候，类型断言也是很常见的：

当我们使用 JavaScript 的类的时候 instanceof 是最好的解决方案

```typescript
class ApiError extends Error {
    code: number = 0;
}
class HttpError extends Error {
    statusCode: number = 200;
}

function isApiError(error: Error) {
    if (error instanceof ApiError) {
        return true;
    }
    return false;
}
```

但是当使用 TypeScript 的接口的时候，接口是一个类型，而不是一个真正的值，它在编译结果中会被删除，当然也就无法使用 instanceof

这时也就只能使用类型断言，通过判断是否存在 code 属性，来判断传入的参数是不是 ApiError 了：

```typescript
interface ApiError extends Error{
    code: number;
}
interface HttpError extends Error {
    statusCode: number;
}

function isApiError(error: Error) {
    if (typeof (error as ApiError).code === 'number') {
        return true;
    }
    return false;
}
```

### 将任何一个类型断言为 any

理想情况下，TypeScript 的类型系统运转良好，每个值的类型都具体而精确。

当我们引用一个在此类型上不存在的属性或方法时，就会报错。

但是有时，当我们非常确定一段代码不会出错的时候，比如下面这个例子

```typescript
window.foo = 1;
// index.ts:1:8 - error TS2339: Property 'foo' does not exist on type 'Window & typeof globalThis';
```

在上面的例子中，我们需要将 window 上添加一个属性 foo，但 TypeScript 编译时会报错，提示我们 window 上不存在 foo 属性

此时我们可以使用 as any 临时将 windiw 断言为 any 类型：

```typescript
(window as any).foo = 1;
```

在 any 类型的变量上，访问任何属性都是允许的

需要注意的是，将一个变量断言为 any 可以说是解决 TypeScript 中类型问题的最后一个手段

**它极有可能覆盖了真正的类型错误，所以如果不是非常确定，就不要使用 as any**

上面的例子中，我们也可以通过 [扩展 window 的类型（TODO）][] 解决这个错误，不过如果只是临时的增加 foo 属性，as any 会更加方便

 一方面不能滥用 as any ，另一方面我们也不能完全否定它的作用，我们需要在类型的严格性和开发的便利性之间掌握平衡，才能发挥出 TypeScript 最大的价值
 
 ### 将 any 断言为一个具体的类型

在日常的开发中，我们不可避免的需要处理 any 类型的变量，它们可能是由于第三方库未能定义好自己的类型，也有可能是历史遗留的或其他人编写的烂代码，
还可能是受到 TypeScript 类型系统的限制而无法精准定义类型的场景

遇到 any 类型的变量时，我们可以选择无视它，任由它滋生更多的 any

我们也可以尝试通过类型断言及时的把 any 断言为精确的类型，亡羊补牢，使我们的代码向着高可维护性的目标发展

举例：

```typescript
function getCacheData(key: string): any {
    return (window as any).cache[key];
}

interface Cat {
    name: string;
    run(): void;
}

const tom = getCacheData('tom') as Cat;
tom.run();
```

### 类型断言的限制

> 前置知识点 [结构类型系统（TODO）][]、[结构兼容性（TODO）][]

从上面的例子中，我们可以总结出以下几点：
* 联合类型可以被断言为其中一个类型
* 父类可以被断言为子类
* 任何类型可以被断言为 any
* any 可以被断言为任何类型

但是类型断言也是有限制的，具体来说：

**若 A 兼容 B，那么 A 能够被断言为 B，B 也能被断言为 A**

```typescript
interface Animal {
    name: string;
}
interface Cat {
    name: string;
    run(): void;
}

let tom: Cat = {
    name: "Tom",
    run() { console.log('run') }
}

let animal: Animal = tom;
```

TypeScript 是结构类型系统，类型之间的对比只会比较它们最终的结构，而会忽略它们定义时的关系

在上面的例子中，Cat 包含了 Animal 中所有的属性，除此之外，它还有一个额外的方法 run 
TypeScript 并不关心 Cat 和 Animal 之间定义时是什么关系，而只会看它们最终的结构有什么关系 —— 所以它与 Cat extends Animal 是等价的

```typescript
interface Animal {
    name: string
}
interface Cat extends Animal{
    run(): void;
}
```

用 TypeScript 中更专业的说法，就是：Animal 兼容 Cat

当 Animal 建通 Cat 时，它们就可以互相进行类型断言了

```typescript
interface Animal {
    name: string;
}
interface Cat {
    name: string;
    run(): void;
}

function testAnimal(animal: Animal) {
    return(animal as Cat)
}
function testCat(cat: Cat) {
    return(Cat as Animal)
}
```

这样设计其实也很容易理解：
* 允许 animal as cat 是因为 父类可以被断言为子类
* 允许 cat as animal 是因为既然子类用于父类的属性和方法，那么被断言为父类，获取父类的属性，调用父类的方法，就不会有任何问题，故 子类可以被断言为父类

注意，这里简化了父类子类的关系来表达类型的兼容性。而实际上 TypeScript 在判断类型的兼容性时，比这种情况复杂很多

总之，若 A 兼容 B，那么 A 能被断言为 B，B 也能被断言为 A

同理，若 B 兼容 A，那么 A 能被断言为 B，B 也能被断言为 A

要使得 A 能被断言为 B，只需要 A 兼容 B 或 B 兼容 A 即可，这也是为了在类型断言时的安全考虑，毕竟毫无依据的断言是非常为危险的

## 双重断言

既然：

* 任何类型都可以被断言为 any
* any 可以被断言为任何类型

那么：

我们是不是可以使用 双重断言 as any as Foo 来将任何一个类型断言为任何另一个类型呢？

** 并不是，若是在使用了双重断言会打破 要使得 A 能够被断言为 B，只需要 A 兼容 B 或 B 兼容 A 即可的限制，
将任何一个类型断言为任何另一个类型，那么这种双重断言十有八九是错误的**

除非迫不得已，千万别使用 双重断言

## 类型断言 vs 类型转换

类型断言只会影响 TypeScript 编译时的类型，类型断言语句在编译结果中会被删除

```typescript
function toBoolean(something: any): boolean {
    return something as boolean;
}

toBoolean(1) // 1
```

上面的代码，将 something 断言为 boolean 虽然可以通过编译，但是并没有什么用，代码在编译后会变成

```typescript
function toBoolean(something) {
    return something;
}

toBoolean(1); // 1
```

所以类型断言不是类型转换，他不会真的影响到变量的类型

若要进行类型转换，需要直接调用类型转化的方法：

```typescript
function toBoolean(something: any): boolean {
    return Boolean(something)
}

toBoolean(1) // true
```

## 类型断言 vs 类型声明

```typescript
function getCacheData(key: string): any {
    return (window as any).cache[key];
}

interface Cat {
    name: string;
    run(): void;
}

const tom = getCacheData('tom') as Cat;
tom.run();
```

上面的例子中，我们使用了 as Cat 将 any 类型断言为了 Cat 类型

但实际上还有其他方式可以解决这个问题：

```typescript
function getCacheData(key: string): any {
    return (window as any).cache[key];
}

interface Cat {
    name: string;
    run(): void;
}

const tom: Cat = getCacheData('tom');
tom.run();
```

上面的例子中，我们通过类型声明的方式，将 tom 声明为 Cat，然后将 any 类型的 getCacheData('tom') 赋值给 Cat 类型的 tom

这和类型断言是非常相似的，而且产生的结果也几乎是一样的 —— tom 在接下来的代码中都变成了 Cat 类型

两者的区别可以通过下面这个例子来理解：

```typescript
interface Animal {
    name: string;
}
interface Cat {
    name: string;
    run(): void;
}

const animal: Animal = {
    name: 'tom'
}

let tom = animal as Cat; // true
let bob: Cat = animal; // error
```

上面的例子中：
* 由于 Animal 兼容 Cat，故可以将 animal 断言为 Cat 赋值给 tom
* 但若直接声明 tom 为 Cat 类型，则会报错，不允许将 animal 赋值给 Cat 类型的 tom，因为 Animal 可以看作是 Cat 的父类，
当然不能将父类的实例赋值给类型为子类的变量

核心区别在于：
* animal 断言为 Cat，只需要满足 Animal 兼容 Cat 或 Cat 兼容 Animal 即可
* animal 赋值给 tom，需要满足 Cat 兼容 Animal 才行，但是 Cat 并不兼容 Animal 

所以 ** 类型声明是比类型断言更加严格的 ** 

为了增加代码的质量，我们最好优先使用类型声明，这也比类型断言的 as 语法更加优雅

## 类型断言 vs 泛型

还是上面这个例子：

```typescript
function getCacheData(key: string): any {
    return (window as any).cache[key];
}

interface Cat {
    name: string;
    run(): void;
}

const tom = getCacheData('tom') as Cat;
tom.run()
```

我们还有更优的解决方案，那就是泛型：

```typescript
function getCacheData<T>(key: string): T {
    return (window as any).cache[key];
}

interface Cat {
    name: string;
    run(): void;
}

const tom = getCacheData<Cat>('tom');
tom.run()
```

通过给 getCacheData 函数添加了一个泛型 <T>，我们可以更加规范的实现对 getCacheData 返回值的约束，这也同时去除掉了代码中的 any，
是最优的解决方案
