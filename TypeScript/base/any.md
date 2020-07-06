# 任意值

> 任意值相当于没用 ts

任意值 Any 用来表示允许赋值为任意类型

如果一个值被定于为 any 类型，则允许被赋值为任意类型

```typescript
let number: any = 'seven';
number = 7;
```

## 任意值的属性和方法

在任意值上访问任何属性都是与允许的，也允许调用任何方法：

```typescript
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);

let anything: any = 'tom';
anyThing.setName('Jetty');
anyThing.setName('Jetty').sayHello();
anyThing.myName.setFirstName('Cat');
```

可以认为，声明一个变量为任意值后，对它的任何操作，返回的内容的类型都是任意值。

## 未声明的变量

如果在声明变量的时候，未指定其类型，那么它会被识别为任意值类型


