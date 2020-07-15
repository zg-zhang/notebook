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

### 关键代码

webpack.config.js

```javascript
const fs = require('fs')
const path = require('path')
const webpack = require('webpack')

module.exports = {
  mode: 'development',

  /**
   * 在 examples 目录下创建多个子目录
   * 把不同章节的 demo 放在不同的子目录下
   * 每个子目录下会创建一个 app.ts
   * app.ts 作为 webpack 构建的入口文件
   * entries 收集了多个目录入口文件，并且每个入口还引入了一个用于热更新的文件
   * entries 是一个对象，key 为目录名
   */
  entry: fs.readdirSync(__dirname).reduce((entries, dir) => {
    const fullDir = path.join(__dirname, dir);
    const entry = path.join(fullDir, 'app.ts')
    if (fs.statSync(fullDir).isDirectory() && fs.existsSync(entry)) {
      entries[dir] = ['webpack-hot-middleware/client', entry]
    }

    return entries
  }, {}),

  /**
   * 根据不同的目录名称，打包生成目标 js，名称和目录名称一致
   */
  output: {
    path: path.join(__dirname, '__build__'),
    filename: '[name].js',
    publicPath: '/__build__/'
  },

  module: {
    rules: [
      {
        test: /\.ts$/,
        enforce: 'pre',
        use: [
          {
            loader: 'tslint-loader'
          }
        ]
      },
      {
        test: /\.tsx?$/,
        use: [
          {
            loader: 'ts-loader',
            options: {
              transpileOnly: true
            }
          }
        ]
      }
    ]
  },

  resolve: {
    extensions: ['.ts', '.tsx', '.js']
  },

  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NoEmitOnErrorsPlugin()
  ]
}
```

server.js

```javascript
const express = require('express')
const bodyParser = require('body-parser')
const webpack = require('webpack')
const webpackDevMiddleware = require('webpack-dev-middleware')
const webpackHotMiddleware = require('webpack-hot-middleware')
const WebpackConfig = require('./webpack.config')

const app = express()
const compiler = webpack(WebpackConfig)
const router = express.Router()

router.get('/simple/get', function(req, res) {
  res.json({
    msg: 'hello world'
  })
})

app.use(router)

app.use(webpackDevMiddleware(compiler, {
  publicPath: '/__build__/',
  stats: {
    colors: true,
    chunks: false
  }
}))

app.use(webpackHotMiddleware(compiler))

app.use(express.static(__dirname))

app.use(bodyParser.json())
app.use(bodyParser.urlencoded({ extended: true }))

const port = process.env.PORT || 8000
module.exports = app.listen(port, () => {
  console.log(`Server listening on http://127.0.0.1:${port}`)
})

```
