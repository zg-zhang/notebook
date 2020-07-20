# 文件监控

gulp api 中的 watch() 方法利用文件系统的监控程序将 globs 与任务进行关联，它对匹配 glob 的文件进行监控，如果有文件被修改了就执行关联的任务

如果被执行的任务没有触发异步完成信号，它将永远不会再次执行了

## 初次执行

调用了 watch() 之后，关联的任务 task 是不会被立即执行的，而是要等到第一次文件修改之后才执行

如需在第一次文件修改之前执行，也就是调用 watch() 之后立即执行，请将 ignoreInitial 参数设置为 false

```javascript
const { watch } = require('gulp');

// 关联的任务（task）将在启动时执行
watch('src/*.js', { ignoreInitial: false }, function(cb) {
  // body omitted
  cb();
});
```
