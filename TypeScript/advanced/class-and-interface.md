# 类与接口

接口可以用于对 对象的形状 进行描述

主要介绍接口的另一个用途，对类的一部分行为进行抽象

## 类实现接口

实现 implements 是面向对象中的一个重要概念，一般来讲，一个类只能继承自另一个类，有时候不同类之间可以有一些共有的特性，
这时候就可以把特性提取成接口，用 implements 关键字来实现，这个特性大大提高了面向对象的灵活性

下面的例子：
* 门是一个类
* 车也是一个类
* 防盗门是门的一个子类
* 防盗门有一个报警器的功能，
* 车也有报警器的功能，
* 我们可以考虑把报警器提取出来，作为一个接口，防盗门和车都去实现它

```typescript
interface Alarm {
    alert(): void;
}

class Door {
  
}

class SecurityDoor extends Door implements Alarm {
    alert() {
        console.log('SecurityDoor alert')
    }
}

class Car implements Alarm{
    alert() {
        console.log('Car alert')
    }
}
```

一个类可以实现多个接口

下例中，Car 实现了 Alarm 和 Light 接口，技能报警，也能开关车灯

```typescript
interface Alarm {
    alert(): void;
}

interface Light {
    lightOn(): void;
    lightOff(): void;
}

class Car implements Alarm, Light {
    alert() {}
    lightOn() {}
    lightOff() {}
}
```

## 接口继承接口

接口与接口之间可以是继承关系：

```typescript
interface Alarm {
    alert(): void;
}

interface LightableAlarm extends Alarm{
    lightOn(): void;
    lightOff(): void;
}
```

上例中，LightAbleAlarm 继承了 Alarm，除了拥有 alert 方法之外，很有用了两个新方法

## 接口继承类

常见的面向对象语言中，接口时不能继承类的，但是在 TypeScript 中却是可以的：

```typescript
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```

实际上，当我们在声明 class Point 时，除了会创建一个名为 Point 的类之外，同时也创建了一个明为 Point 的类型（实例的类型）

所以我们既可以将 point 当作一个类来用 （使用 new Point 创建它的实例）

```typescript
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

function printPoint(p: Point) {
    console.log(p.x, p.y);
}

printPoint(new Point(1, 2));
```

上面的代码等价于下面的

```typescript
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

interface PointInstanceType {
    x: number;
    y: number;
}

function printPoint(p: PointInstanceType) {
    console.log(p.x, p.y);
}

printPoint(new Point(1, 2));
```

上例中我们新声明的 PointInstanceType 类型，与声明 class Point 时创建的 Point 类型是等价的

也就是说 接口继承类 和 接口继承接口 没有什么本质的区别
