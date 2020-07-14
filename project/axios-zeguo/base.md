# 基础请求代码的编写

目标是实现简单的发送请求的功能，即客户通过 XMLHttpRequest 对象把请求发送到 server 端，server 端能收到请求并响应即可

## demo 编写

利用 Node.js 的 express 库去运行 demo，利用 webpack 来作为 demo 的构建工具

### 依赖

* webpack：打包构建工具
* webpack-dev-middleware：express 的 webpack 中间件
* webpack-hot-middleware：express 的 webpack 中间件 
* ts-loader：webpack 需要的 TypeScript 相关 loader
* tslint-loader：webpack 需要的 TypeScript 相关 loader
* express：node.js 的服务端框架
* body-parser：express 中间件，解析 body 数据用
