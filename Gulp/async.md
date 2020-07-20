# 异步执行

## 任务完成通知

当任务返回 stream、promise、event emitter、child process、observable 时，成功或错误值将通知 gulp 是否继续执行或结束

如果任务出错，gulp 将立即结束执行并显示该错误

* 当使用 series() 组合多个任务时，任何一个任务的错误将导致整个任务组合结束，并且不会进一步执行其他任务
* 当 parallel() 组合多个任务时，一个任务的错误将结束整个任务组合的结束，但是其他任务可能会执行完，也可能没有执行完

### 返回 stream

```javascript
const {src, dest} = require('gulp');

function streamTask() {
  return src('*.js')
    .pipe(dest('output'))
}

export.default = streamTask;
```
### 返回 promise

```javascript
function promiseTask() {
  return Promise.resolve('the value is ignored');
}

export.default = promiseTask
```

### 返回 event emitter

```javascript
const { EventEmitter } = require('events');

function eventEmitterTask() {
  const emitter = new EventEmitter();
  setTimeout(() => emitter.emit('finish'), 250);
  return emitter;
}

export.default = eventEmitterTask
```

### 返回 child process

```javascript
const { exec } = require('child_process');

function childProcessTask() {
  return exec('date');
}

export.default = childProcessTask
```

### 返回 observable

```javascript
const { Observable } = require('rxjs');

function observableTask() {
  return Observable.of(1, 2, 3);
}

export.default = observableTask;
```

### 使用 callback

如果任务不返回任何内容，则必须使用 callback 来指示任务已完成

## gulp 不再支持同步任务 Synchronous tasks

gulp 不再支持同步任务了，因为同步任务常常会导致难以调试的细微错误

当你看到 Did you forget to signal async completion? 警告时，说明你未使用前面提到的返回方式

你需要使用 callback 或返回 stream、promise、event emitter、child process、observable 来解决此问题

## 使用 async/await

如果不使用前面提供的几种方式，你还可以将任务定义为一个 async 函数，它将利用 promise 对你的任务进行包装，这将允许你使用 await 处理 promise，并使用其他同步代码

```javascript
const fs = require('fs');

async function asyncAwaitTask() {
  const { version } = fs.readFileSync('package.json');
  console.log(version);
  await Promise.resolve('some result')
}

export.default = asyncAwaitTask;
```

