# 类

传统方法中，JavaScript 通过构造函数实现类的概念，通过原型链实现继承

在 ES6 中，我们终于迎来了 class

## 类的概念

* 类 Class：定义了一件事物的抽象特点，包含它的属性和方法
* 对象 Object：类的实例，通过 new 生成
* 面向对象 OOP 的三大特性：封装、继承、多态
* 封装 Encapsulation：将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节。
就能通过对外提供的接口来访问该对象，同时也保证了外界无法任意更改对象内部的数据。
* 继承 Inheritance：子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体特性
* 多态 Polymorphism：由继承而产生了相关的不同的类，对同一个方法可以有不同的响应。比如：
Cat 和 Dog 都继承 Animal，但是分别实现了自己的 eat 方法，针对某一个实例，我们无需了解它是 Cat 还是 Dog 就可以直接调用 eat 方法，程序会自动判断出来应该如何执行 eat
* 存取器 getter & setter：用以改变属性的读取和赋值行为
* 修饰符 Modifiers：修饰符是一些关键字，用于限定成员或类型的性质，比如 public 表示公有属性或方法
* 抽象类 Abstract Class：抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现
* 接口 Interfaces：不同类之间公有的属性或方法，可以抽象成一个接口。接口可以被类实现 implements。一个类只能继承自另一个类，但是可以实现多个接口。

## ES6 中类的用法

### 属性和方法

使用 class 定义类，使用 constructor 定义构造函数

通过 new 生成新实例的时候，会自动调用构造函数

```typescript
class Animal {
    public name;
    constructor(name) {
        this.name = name;
    }
    sayHi() {
        return `My name is ${this.name}`;
    }
}

let a = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack
```

### 类的继承

使用 extends 关键字实现继承，子类中使用 super 关键字来调用父类的构造函数和方法

```typescript
class Animal {
    public name;
    constructor(name) {
        this.name = name;
    }
    sayHi() {
        return `My name is ${this.name}`;
    }
}

class Cat extends Animal {
    constructor(name) {
        super(name); // 调用父类的 constructor(name)
        console.log(this.name);
    }
    sayHi() {
        return 'Meow, ' + super.sayHi(); // 调用父类的 sayHi()
    }
}

let c = new Cat('Tom'); // Tom
console.log(c.sayHi()); // Meow, My name is Tom
```

### 存取器

使用 getter 和 setter 可以改变属性的赋值和读取行为：

```typescript
class Animal {
    constructor(name) {
        this.name = name;
    }
    get name() {
        return 'Jack';
    }
    set name(value) {
        console.log('setter: ' + value);
    }
}

let a = new Animal('Kitty'); // setter: Kitty
a.name = 'Tom'; // setter: Tom
console.log(a.name); // Jack
```

### 静态方法

使用 static 修饰符修饰的方法称为静态方法，它们不需要实例化，而是直接通过类来调用

```typescript
class Animal {
    static isAnimal(a) {
        return a instanceof Animal
    }   
}

let a = new Animal('Jack');
Animal.isAnimal(a); // true
a.isAnimal(a); // a.isAnimal is not a function 
```

## ES7 中类的用法

ES7 中有一些关于类的提案，TypeScript 也实现了它们

### 实例属性

ES6 中实例的属性只能通过构造函数中的 this.xxx 来定义，ES7 提案中可以直接在类里面的定义

```typescript
class Animal {
    name = 'Jack';
    
    constructor() {
        // ...
    }
}

let a = new Animal();
console.log(a.name); // Jack
```

### 静态属性

ES7 提案中，可以使用 static 定义了一个静态属性

```typescript
class Animal {
    static num = 42;
    
    constructor() {
        // ...
    }
}

console.log(Animal.num) // 42
```

## TypeScript 中类的用法

### public private 和 protected

TypeScript 可以使用三种访问修饰符 Access Modifiers，分别是 public、private、protected

* public 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的
* private 修饰的属性或方法是私有的，不能在声明它的类的外部访问
* protected 修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的

下面举一些例子：

```typescript
class Animal {
    public name;
    public constructor(name) {
        this.name = name
    }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom';
console.log(a.name); // Tom
```

上面的例子中，name 被设置为了 public，所以直接访问实例的 name 属性是允许的

很多时候，我们希望有的属性是无法直接存取的，这时候就可以使用 private 了

```typescript
class Animal {
    private name;
    public  constructor(name) {
        this.name = name;
    }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom'; // error
```

需要注意的是，TypeScript 编译只会的代码中，并没有限制 private 属性的外部的可访问性

上面的例子编译完之后是：

```typescript
var Animal = (function() {
    function Animal(name) {
        this.name = name;
    }
    return Animal;
})();
var a = new Animal('Jack');
console.log(a.name);
a.name = 'Tom'
```

使用 private 修饰的属性和方法，在子类中也是不允许访问的

而如果是用 protected 修饰，则允许在子类中访问

当构造函数修饰为 private 时，该类不允许被继承或者实例化

当构造函数修饰为 protected 时，该类只允许被继承

### 参数属性

修饰符和 readonly 还可以使用在构造函数参数中，等同于类中定义该属性同时给该属性赋值，使代码更加简洁

```typescript
class Animal {
    // public name: string
    public constructor(public name) {
        // this.name = name; 
    }
}
```

### readonly 

只读属性关键字，只允许出现在属性声明或索引签名或构造函数中

```typescript
class Animal {
    readonly name;
    public constructor(name) {
        this.name = name;
    }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom'; // error
```

如果 readonly 和其他访问修饰符同时存在的话，需要写在其后面

```typescript
class Animal {
    public constructor(public readonly name) {}
}
```

### 抽象类

abstract 用于定义抽象类和其中的抽象方法

* 抽象类是允许被实例化的
* 抽象类中的抽象方法必须被子类实现

正确的使用方法：

```typescript
abstract class Animal {
    public name;
    public constructor(name) {
        this.name = name
    }
    public abstract sayHi();
}

class Cat extends Animal{
    public sayHi() {
        console.log(`Meow, My name is ${this.name}`);
    }
}

let cat = new Cat('Tom')
```

注意，虽然是抽象方法，但是 TypeScript 的编译结果中，仍然会存在这个类

## 类的类型

给类加上 TypeScript 的类型很简单，与接口类似：

```typescript
class Animal {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    sayHi(): string {
        return `My name is ${this.name}`;
    }
}

let a: Animal = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack
```

