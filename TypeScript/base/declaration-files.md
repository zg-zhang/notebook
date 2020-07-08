# 声明文件

当我们使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能

* 索引
    * [declare var](#declare-var) 声明全局变量
    * [declare function](#declare-function) 声明全局方法
    * [declare class](#declare-class) 声明全局类
    * [declare enum](#declare-enum) 声明全局枚举类型
    * [declare namespace](#declare-namespace) 声明（含有子类型的）全局对象
    * [interface 和 type](#interface--type) 声明全局类型
    * [export](#export) 导出变量
    * [export namespace](#export-namespace) 导出（含有子属性的）对象
    * [export default](#export-default) ES6 默认导出
    * [export =](#export-) commonjs 导出模块
    * [export as namespace](#export-as-namespace) UMD 库声明全局变量
    * [declare global](#declare-global) 扩展全局变量
    * [declare module](#declare-module) 扩展模块
    * [/// <reference />](#) 三斜线指令

## 什么是声明语句

假象我们需要使用第三方库 jQuery，通常我们在 html 中使用 `<script>` 标签引入，然后就可以全局使用 $ 或者 jQuery 变量了

但是在 TypeScript 中，编译器并不知道 $ 和 jQuery 是什么东西

这时我们需要使用 declare var 来定义它的类型

```typescript
declare var jQuery: (selector: string) => any;

jQuery('#foo');
```

上例中，declare var 并没有真正的定义一个变量，只是定义了全局变量 jQuery 的类型，仅仅用于编译时的检查，在编译结果中会被删除，上例的编译结果为：

```typescript
jQuery('#foo');
```

## 什么是声明文件

通常我们会把声明语句放到一个单独的文件（jQuery.d.ts）中，这就是声明文件

```typescript
// src/jQuery.d.ts

declare var jQuery: (selector: string) => any;

// src/index.ts

jQuery('#foo');
```

声明文件必须要以 `.d.ts` 为后缀

一般来说，ts 会解析项目中所有的 `*.ts` 文件，当然也包括以 `.d.ts` 为后缀的文件。所以当我们将 jQuery.d.ts 放到项目中时，
其他所有项目 *.ts 文件都可以获得 jQuery 的类型定义了

假如仍然无法解析，那么可以检查下 tsconfig.json 中的 files、include 和 exclude 配置，确保其包含了 jQuery.d.ts 文件

### 第三方声明文件

当然我们不需要手写 jQuery 的声明文件，社区已经帮我们定义好了

更推荐使用 `@types` 统一管理第三方库的声明文件

直接用 npm 安装对应的声明模块即可，以 jQuery 举例：

```bash
npm install @types/jquery --save-dev
```

## 书写声明文件

当一个第三方库没有提供声明文件时，我们就需要自己书写声明文件了，前面只介绍了最简单的声明文件内容

在不同的场景下，声明文件的内容和使用方式会有所区别

库的使用场景主要有以下几点

* [全局变量](#全局变量)：通过 `<script>` 标签引入第三方库，注入全局变量
* [npm包](#npm包)：通过 import foo from 'foo' 导入，符合 ES6 模块规范
* [UMD库](#UMD库)：既可以通过 `<script>` 标签引入，又可以通过 `import` 导入
* [直接扩展全局变量](#直接扩展全局变量)：通过 `<script>` 标签引入后，改变一个全局变量的结构
* [在 npm 包或 UMD 库中扩展全局变量](#)：引用 npm 包或 UMD 库后，改变一个全局变量的结构
* [模块插件](#模块插件)：通过 `<script>` 或 `import` 导入后，改变另一个模块的结构

### 全局变量

使用全局变量的声明文件时，如果是以 `npm install @types/xxx --save-dev` 安装的，则不需要任何配置。

如果是将声明文件直接存放于当前项目中，则建议和其他源码一起放在 src 目录下（或者对应的源码目录下）：

```
/path/to/project
├─ src
│   ├─ index.ts
│   └─ jQuery.d.ts
└─ tsconfig.json
```

如果没有生效，可以检查下 tsconfig.json 中的 files、include、exclude 配置，确保其包括了 jQuery.d.ts 文件

全局变量的声明文件主要有以下几种语法：
* [declare var](#declare-var) 声明全局变量
* [declare function](#declare-function) 声明全局方法
* [declare class](#declare-class) 声明全局类
* [declare enum](#declare-enum) 声明全局枚举类型
* [declare namespace](#declare-namespace) 声明（含有子类型的）全局对象
* [interface 和 type](#interface--type) 声明全局类型

#### declare var 

```typescript
declare const jQuery: (selector: string) => any;
```

能够用来定义一个全局变量的类型，与其类似的还有 `declare let` 和 `declare const` 使用 let 和 var 没有什么区别

使用 declare var / declare let 定义全局变量允许修改，而使用 declare const 定义的全局变量是常量，不允许修改

一般来说全局变量都应该是常量，是禁止修改的，所以应该使用 const 而不是 let 和 var

** 注意，声明语句只能定义类型，切勿在声明语句中定义具体的实现 **

#### declare function

declare function 用来定义全局函数的类型，jQuery 其实就是一个函数，所以也可以用 function 来定义

在函数类型的声明中，函数重载也是支持的

```typescript
declare function jQuery(selector: string): any;
declare function jQuery(domReadyCallback: () => any): any;

jQuery('#foo');
jQuery(function() {
    alert('Dom Ready!');
})
```

#### declare class

当全局变量是一个类的时候，我们使用 declare class 来定义它的类型

```typescript
declare class Animal {
    name: string;
    constructor(name: string);
    sayHi(): string;
}

let cat = new Animal('Tom')
```

同样只能声明，不能有具体实现

#### declare enum

使用 declare enum 定义的枚举类型也称做外部枚举 (Ambient Enums)

```typescript
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

同样仅仅用于定于类型，而不是具体的值

#### declare namespace

用来表示全局变量是一个对象，包含很多子属性

比如 jQuery 是一个全局变量，它是一个对象，提供了一个 jQuery.ajax 方法可以调用，那么我们就应该使用 declare namespace jQuery 来声明这个拥有多个子属性的全局变量

```typescript
declare namespace jQuery {
    function ajax(url: string, setting?: any): void;
}

jQuery.ajax('/api/get_something');
```

注意，在 declare namespace 内部，我们直接使用 function ajax 来声明函数，而不是使用 declare function ajax

类似，也可以使用 从 const、class、enum 等语句：

```typescript
declare namespace jQuery {
    function ajax(url: string, setting?: any): void;
    const version: number;
    class Event {
        blur(eventType: EventType): void
    }
    enum EventType {
        CustomClick
    }
}

jQuery.ajax('/api/get_something');
console.log(jQuery.version);
const e = new jQuery.Event();
e.blur(jQuery.EventType.CustomClick);
```

#### 嵌套的命名空间

如果对象拥有深层的层级，则需要用嵌套的 namespace 来声明深层的属性的类型

```typescript
declare namespace jQuery {
    function ajax(url: string, setting?: any): void;
    namespace fn {
        function extend(object: any): void;
    }
}

jQuery.ajax('/api/get_something');
jQuery.fn.extend({
    check: function() {
        return this.each(function() {
            this.checked = true;
        })
    }
}) 
```

当 jQuery 下仅有 fn 这一个属性 没有 ajax 等其他属性或方法，则不需要嵌套 namespace

```typescript
declare namespace jQuery.fn {
    function extend(object: any): void;
}
```

#### interface 和 type

除了全局变量之外，可能有一些类型我们也希望能暴漏出来，在类型声明文件中，我们可以直接使用 interface 或 type 来声明一个全局的接口或类型

这样的话，在其他文件中也可以使用这个接口或类型了：

```typescript
interface AjaxSettings {
    method?: 'GET' | 'POST';
    data?: any;
}
declare namespace jQuery {
    function ajax(url: string, setting?: AjaxSettings): void;
}

let settings: AjaxSettings = {
    method: 'POST',
    data: {
        name: 'foo'
    }
};
jQuery.ajax('/api/get_something',settings);
```

#### 防止命名冲突

暴露在最外层的 interface 和 type 会作为全局类型作用域整个项目中，我们应该尽可能的减少全局变量或全局类型的数量，故最好将他们放在 namespace 下：

```typescript
declare namespace jQuery {
    interface AjaxSettings {
        method?: 'GET' | 'POST';
        data?: any;
    }
    function ajax(url: string, setting?: AjaxSettings): void;   
}

// 注意，这里使用 interface 要加上 namespace 前缀
let settings: jQuery.AjaxSettings = { 
    method: 'POST',
    data: {
        name: 'foo'
    }
}
jQuery.ajax('/api/post_something', settings);
```

#### 声明合并

比如 jQuery 既是一个函数，可以直接调用 jQuery('#foo')，有是一个对象，拥有子属性 jQuery.ajax()

那么可以组合多个声明语句，他们会不冲突的合并起来：

```typescript
declare function jQuery(selector: string): any;
declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}
```

### npm包

一般我们通过 `import foo from 'foo'` 导入一个 npm 包，这是符合 ES6 模块规范的

在我们尝试给一个 npm 包创建声明文件之前，需要先看看它的声明文件是否已经存在

一般来说，npm包的声明文件可能存在于两个地方：
* 与该 npm 包绑定在一起，package.json 中有 types 字段，或者有一个 index.d.ts 声明文件，这种模式不需要安装其他包，是最为推荐的
* 发布到 @types 里，我们只需要尝试安装一下对应的 @types 包就知道是否存在该声明文件，这种模式一般是由于 npm 包维护者没有提供声明文件，只能由其他人将声明文件发布到 @types 里了

假如以上两种方式都没有找到对应的声明文件，那么我们就需要自己为它写声明文件了，由于是通过 import 语句导入的模块，所以声明文件存放的位置也有所约束，一般有两种方案
* 创建一个 node_modules/@types/foo/index.d.ts 文件，存放 foo 模块的声明文件，不需要额外配置，但是 node_modules 目录不稳定，代码也没有被保存到仓库中，无法回溯版本，有不小心删除的风险，一般只有测试的时候才使用这种方法
* 创建一个 types 目录，专门用来管理自己写的声明文件，将 foo 的声明文件放到 types/foo/index.d.ts 中，这种方式需要配置下 tsconfig.json 中的 paths 和 baseUrl

```
├─ src
│   └─ index.ts
├─ types
│   └─ foo
│       └─ index.d.ts
└─ tsconfig.json
```

```json
{
    "compilerOptions": {
        "module": "commonjs",
        "baseUrl": "./",
        "paths": {
            "*": ["types/*"]
        }
    }
}
```

npm 包的声明文件主要有以下几种语法：

* [export](#export) 导出变量
* [export namespace](#export-namespace) 导出（含有子属性的）对象
* [export default](#export-default) ES6 默认导出
* [export =](#export-) commonjs 导出模块

#### export

npm 包的声明文件和全局变量的声明文件有很大区别。

在 npm 包的声明文件中，使用 declare 不再会声明一个全局变量，而只会在当前文件中声明一个局部变量

只有在声明文件中使用 export 导出，任何在使用方 import 导入后，才会应用到这些类型声明

export 的语法和普通的 ts 中的语法类似，区别仅在于声明文件中禁止定义具体的实现

```typescript
// types/foo/index.d.ts
export const name: string;
export function getName(): string;
export class Animal {
    constructor(name: string);
    sayHi(): string;
}
export enum Directions {
    Up,
    Down,
    Left,
    Right
}
export interface Options {
    data: any;
}
```

#### 混用 declare 和 export

我们也可以使用 declare 先声明多个变量，最后再用 export 一次性导出，上例的声明文件可以等价的改写为：

```typescript
declare const name: string;
declare function getName(): string;
declare class Animal {
    constructor(name: string);
    sayHi(): string
};
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}
interface Options {
    data: any;
}

export { name, getName, Animal, Directions, Options };
```

** 注意，与全局变量的声明文件类似，interface 前是不需要 declare 的 ** 

#### export namespace

与 declare namespace 类似，export namespace 用来导出一个拥有子属性的对象

```typescript
export namespace foo {
    const name: string;
    namespace bar {
        function baz(): string;
    }
}
``` 

#### export default

在 ES6 模块系统中，使用 export default 可以导出一个默认值，使用方可以用 import foo from 'foo' 而不是 import {foo} from 'foo' 来导出这个默认值

在类型声明文件中，export default 用来导出默认值的类型

```typescript
export default function foo():string;
```

** 注意，只有 function、class、interface 可以直接默认导出，其他的变量需要先定义出来，再默认导出

```typescript
export default Directions;

declare enum Directions {
    Up,
    Down,
    Left,
    Right
}
```

针对上面这种默认导出，我们一般会把导出语句放在整个声明文件的最前面

#### export =

在 commonjs 规范中，我们用以下方法来导出一个模块：

```javascript
// 整体导出
module.exports = foo;

// 单个导出
exports.bar = bar;
```

在 ts 中，针对这种模块导出，有很多种方式可以导入

第一种是 const ... = require

```typescript
const foo = require('foo');

const bar = require('foo').bar;
```

第二种是 import ... from 注意针对整体导出，需要使用 import * as 来导入

```typescript
import * as foo from 'foo';

import { bar } from 'foo';
```

第三种方式是 import ... require ，这也是 ts 官方推荐的方式

```typescript
import foo = require('foo');

import bar = foo.bar;
```

对于这种使用 commonjs 规范的库，假如要为它写类型声明文件的话，就需要使用到 export = 这种语法了

```typescript
export = foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}
```

上例中使用了 export = 之后，就不能再单个导出 export { bar }，所以我们通过声明合并，使用 declare namespace foo 来将 bar 合并到了 foo 里

### UMD 库

既可以通过 `<script>` 标签引入，也可以通过 import 导入的库，称为 UMD 库，相比较 npm 包的类型声明文件，我们需要额外声明一个全局变量

为了实现这种方法，ts 提供了一个新的语法： export as namespace

#### export as namespace

一般使用 export as namespace 时，都是先有了 npm 包的声明文件，再基于它添加一条 export as namespace 语句，即可将声明好的一个变量声明为全局变量

```typescript
export as namespace foo;
export = foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}
```

它也可以和 export default 一起使用

```typescript
export as namespace foo;
export default foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}
```

### 直接扩展全局变量

有的第三方库扩展了一个全局变量，可是此全局变量的类型却没有相应的更新过来，就会导致 ts 编译错误，此时就需要扩展全局变量的类型，比如扩展 String 类型

```typescript
interface String {
    prependHello(): string;
}

'foo'.prependHello();
```

通过声明合并，使用 interface String 即可给 String 添加属性或方法

也可以使用 declare namespace 给已有的命名空间添加类型声明

```typescript
declare namespace JQuery {
    interface CustomOptions {
        bar: string;
    }
}

interface JQueryStatic {
    foo(options: JQuery.CustomOptions): string;
}

JQuery.foo({
    bar: ''
})
``` 

### 在 npm 包或 UMD 库中扩展全局变量

如之前所说，对于一个 npm 包或者 UMD 库的声明文件，只有 export 导出的类型声明才能被导入

所以对于 npm 包或者 UMD 库，如果导入此库之后会扩展全局变量，则需要使用另一种语法在声明文件中扩展全局变量的类型，那就是 declare global

#### declare global

使用 declare global 可以在 npm 包或者 UMD 库的声明文件中扩展全局变量的类型

```typescript
declare global {
    interface String {
        prependHello(): string;
    }
}

export {};

'bar'.prependHello();
```

注意，及时此声明文件不需要导出任何东西，仍需导出一个空对象，用来告诉编译器这是一个模块的声明文件，而不是一个全局变量的声明文件

### 模块插件

有时通过 import 导入一个模块插件，可以改变另一个原有模块的结构，此时如果原有模块已经有了类型声明文件，而插件模块没有声明文件，就会导致类型不完整，缺少插件部分的类型

ts 提供了一个语法 declare module，它可以用来扩展原有模块的类型

#### declare module

如果是需要扩展原有模块的话，需要在类型声明文件中先引用原有模块，再使用 declare module 扩展原有模块

```typescript
// types/moment-plugin/index.d.ts
import * as moment from 'moment';

declare module 'moment' {
    export function foo(): moment.CalendarKey;
}

// src/index.ts
import * as moment from 'moment';
import 'moment-plugin';

moment.foo();
```

declare module 也可以用于在一个文件中一次性声明多个模块的类型：

```typescript
// types/foo-bar.d.ts

declare module 'foo' {
    export interface Foo {
        foo: string;
    }
}

declare module 'bar' {
    export function bar(): string;
}

// src/index.ts

import { Foo } from 'foo';
import * as bar from 'bar';

let f: foo;
bar.bar();
```

### 声明文件中的依赖

一个声明文件有时会依赖另一个声明文件中的类型

除了可以在声明文件中通过 import 导入另一个声明文件中的类型之外，还有一个语法也可以用来导入另一个声明文件，那就是三斜线指令

#### 三斜线指令

三斜线与 import 的区别是，当且仅当在以下几个场景下，我们才需要使用三斜线指令代替 import：

* 当我们在书写一个全局变量的声明文件时
* 当我们需要依赖一个全局变量的声明文件时

#### 书写一个全局变量的声明文件

在全局变量的声明文件中，是不允许出现 import、export 关键字的，一旦出现了，那么他就会被视为一个 npm 包或 UMD 库，就不再是全局变量的声明文件了

所以我们在书写一个全局变量的声明文件时，如果需要引用另一个库的类型，那么就必须用三斜线指令了

```typescript
/// <reference types="jquery" />

declare function foo(options: JQuery.AjaxSettings): string;

foo({});
```

三斜线指令的语法如上：/// 后面使用 xml 的格式添加了对 jquery 类型的依赖，着样就可以在声明文件中使用 JQuery.Ajaxsettings 类型了

注意，三斜线指令必须放在文件的最顶端，三斜线指令的前面只允许出现单行或多行注释

#### 依赖一个全局变量的声明文件

```typescript
/// <reference types="node" />

export function foo(p: NodeJS.Process): string;
  
import { foo } from 'node-pligin';

foo(global.process)
```

除了上面两种情况，其他情况都使用 import 进行导入

#### 拆分声明文件

当我们的全局变量的声明文件太大时，可以通过拆分为多个文件，然后在一个入口文件中将它们一一引入，来提高代码的可维护性

下面就是 jQuery 的声明文件：

```typescript
// node_modules/@types/jquery/index.d.ts

/// <reference types="sizzle" />
/// <reference path="JQueryStatic.d.ts" />
/// <reference path="JQuery.d.ts" />
/// <reference path="misc.d.ts" />
/// <reference path="legacy.d.ts" />

export = jQuery;
```

其中用到了 types 和 path 两种不同的指令，它们的区别是：types 用于声明对一个库的依赖，而 path 用于声明对另一个文件的依赖

#### 其他三斜线指令

除了这两种三斜线指令外，还有其他的三斜线指令，但是都废除了

### 自动生成声明文件

如果库的源码本身就是由 ts 写的，那么在使用 tsc 脚本将 ts 编译为 js 的时候，添加 declaration 选项，就可以同时也生成 .d.ts 声明文件了

我们可以在命令行里添加 --declaration (简写 -d)，或者在 tsconfig.json 中添加 declaration 选项

```json
{
    "compolerOptions": {
        "module": "commonjs",
        "outDir": "lib",
        "declaration": true
    }
}
```

上面我们添加了 outDir 选项，将 ts 文件的编译结果输出到 lib 目录下， 将 declaration 设置为 true 表示将会生成 .d.ts 声明文件，也会输出到 lib 目录下

自动生成其实就是基本保持源码的结构，而将具体实现去掉了，生成了对应的类型声明

这样的好处是，不仅可以在使用 import foo from 'foo' 导入默认的模块时获得类型提示，也可以在使用 import bar from 'foo/lib/bar' 导入一个子模块时，也获得类型提示

其他几个关于自动生成声明文件的配置选项
* declarationDir：设置生成 .d.ts 文件的目录
* declarationMap：对每个 .d.ts 文件，都生成对应的 .d.ts.map（sourcemap）文件
* emitDeclarationOnly：仅生成 .d.ts 文件，不生产 .js 文件
