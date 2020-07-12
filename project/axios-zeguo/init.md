# 初始化项目

> 在这里使用 [TypeScript library starter](https://github.com/alexjoverm/typescript-library-starter)

## TypeScript library starter

这是一个开源的 TypeScript 开发基础库的脚手架工具，可以帮助我们快速初始化一个 TypeScript 项目

### 使用方式

```bash
git clone https://github.com/alexjoverm/typescript-library-starter.git axios-zeguo
cd axios-zeguo

npm install
```

### 关键目录文件介绍

```
├─ src // 源码目录
├─ test // 测试目录
├─ tools // 发布到 GitHub pages 以及发布到 npm 的一些配置脚本工具
├─ tsconfig.json // TypeScript 编译配置文件
├─ tslint.json // TypeScript lint 文件
└─ rollup.config.ts // rollup 配置文件
```

### 优秀工具集成

在 TypeScript library starter 创建的项目集成了很多优秀的开源工具：

* RollupJS：打包
* Prettier 和 TSLint：格式化代码以及保证代码风格一致性
* TypeDoc：自动生成文档并部署到 Github pages
* Jest：单元测试
* Commitizen：生成规范化的提交注释
* Semantic release：管理版本和发布
* husky：更简单的使用 git hooks
* Conventional changelog：通过代码提交信息自动生成 change log

