# 内置对象

JavaScript 中有很多的内置对象，它们可以直接在 TypeScript 中当作定义好了的类型

内置对象是根据标准在全局作用域 Global 上存在的对象，这里的标准是 ECMAScript 和其他环境 比如 DOM 的标准

## TypeScript 核心库的定义文件

地址：[TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/master/src/lib)

它定义了所有浏览器环境需要用到的类型，并且是预制在 TypeScript 中的

注意 TypeScript 核心库的定义中不包括 Node.js 部分

## 用 TypeScript 写 Node.js

Node.js 不是内置对象的一部分，如果想用 TypeScript 写 Node.js 则需要引入第三方声明文件

```bash
npm install @types/node --save-dev
```
