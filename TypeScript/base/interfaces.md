# 对象的类型 —— 接口

在 TypeScript 中，我们使用接 Interface 口来定义对象的类型

## 什么是接口？

在面向对象语言中，接口是一个很重要的概念，他是对行为的抽象，而具体如何行动需要由类 classes 去实现 implement

TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分进行抽象以外，也常用于对 对象的形状 shape 进行描述

## 简单的例子

```typescript
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 22
}
```

上面的代码，我们就定义了一个接口 Person，接定义了一个变量 tom， 它的类型是 Person。这样就约束了 tom 的形状必须和接口 Person 一致

定义的变量比接口多一些或者少一些属性都是不被允许的，可见，赋值的时候，变量的形状必须和接口的形状保持一致

## 可选属性

有时我们希望不要完全匹配一个形状，那么可以用可选属性：

可选属性的含义是该属性可以不存在，这时仍然不允许添加未定义的属性

```typescript
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom'
}

let bob: Person = {
    name: 'Bob',
    age: 23
}
```

## 任意属性

有时我们希望一个接口有任意的属性，可以使用如下的方法：

```typescript
interface Person {
    name: string;
    age?: number;
    [propName: string]: string
}

let tom: Person = {
    name: 'Tom',
    gener: 'male'
}
```

使用了 `[propName: string]: string` 定义了任意属性取 string 类型的值

需要注意的是，一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集

上面的代码，任意属性的值允许是 string 但是可选属性 age 的值却是 number，number 不是 string 的子集，所以报错了

要是想正常使用可以改成如下代码

```typescript
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number;
}

let tom: Person = {
    name: 'Tom',
    age: 24,
    gener: 'male'
}
```

## 只读属性

有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以使用 `readonly` 定义只读属性

**注意，只读的约束只存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**

```typescript
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 8972,   
    name: 'tom',
    age: 25,
    gener: 'male'
}

tom.id = 8221 // error
```


