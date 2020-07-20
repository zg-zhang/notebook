# 创建任务

每个 gulp 任务都是一个异步的 JavaScript 函数，此函数是一个可以接收 callback 作为参数的函数

## 导出任务

任务 tasks 可以是 public 或者是 private 类型的

* public tasks: 从 gulpfile 中被导出 export，可以通过 gulp 命令直接调用
* Private tasks: 被设计为在内部使用，通常作为 series() 或 parallel() 组合的组成部分

一个私有类型的任务在外观和行为上和其他任务是一样的，但是不能被用户直接调用

如需将一个任务注册为公开类型的，只需从 gulpfile 中导出 export 即可

## 组合任务

Gulp 提供了两个强大的组合方法：series() 和 parallel()，允许将多个独立的任务组合为一个更大的操作

这两个方法都可以接受任意数目的任务函数或已经组合的操作，并且 series() 和 parallel() 可以互相嵌套至任意深度

* 如果需要让任务按顺序执行，需要使用 series() 方法
* 如果希望以最大并发运行的任务，可以使用 parallel() 方法将它们组合起来

当一个组合操作执行时，这个组合中的每一个任务每次被调用时都会被执行
