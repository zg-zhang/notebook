# 声明合并

如果定义了两个相同名字的函数、接口和类，那么它们会合并成一个类型

## 函数的合并

我们可以使用重载定义多个函数类型：

```typescript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

## 接口的合并

接口中的属性在合并时会简单的合并到一个接口中：

```typescript
interface Alarm {
    price: number;
}
interface Alarm {
    weight: number;
}
```

相当于：

```typescript
interface Alarm {
    price: number;
    weight: number;
}
```

* 注意，合并的属性的类型必须是唯一的
    * 如果重复了，但是类型一样，这样不会报错
    * 类型不一样，就会报错

接口中方法的合并和函数的合并是一样的

## 类的合并

类的合并与接口的合并规则一定
