# 处理文件

gulp 暴露了 src() 和 dest() 方法用于处理计算机上存放的文件

src() 接受 glob 参数，并从文件系统中读取文件然后生成一个 Node流 stream ，它将所有匹配的文件读取到内存中并通过流 stream 进行处理

stream 流所提供的主要的 API 是 .pipe() 方法，用于连接转换流或可写流

```javascript
const { src, dest } = require('gulp');
const babel = require('gulp-babel');

exports.default = function() {
  return src('src/*.js')
    .pipe(babel())
    .pipe(dest('output/'));
}
```

dest() 接受一个输出目录作为参数，并且它还会产生一个流，通常作为终止流。

当它接收到通过管道传输的文件时，它会将文件内容及文件属性写入到指定的目录中。

gulp 还提供了 symlink() 方法，其操作方式类似 dest() 但是创建的是链接而不是文件

大多是情况下，利用 .pipe() 方法将插件放置在 src() 和 dest() 之间，并转换流中的文件

## 向流中添加文件

src() 也可以放在管道中间，以根据给定的 glob 向流中添加文件。

新加入的文件只对后续的转换可用。

如果 glob 匹配的文件与之前的有重复，仍然会再次添加文件

```javascript
const { src, dest } = require('gulp');
const babel = require('gulp-babel');
const uglify = require('gulp-uglify');

exports.default = function() {
  return src('src/*.js')
    .pipe(babel())
    .pipe(src('vendor/*.js'))
    .pipe(uglify())
    .pipe(dest('output/'));
}
```

## 分阶段输出

dest() 可以用在管道中间用于将文件的中间状态写入文件系统

```javascript
const { src, dest } = require('gulp');
const babel = require('gulp-babel');
const uglify = require('gulp-uglify');
const rename = require('gulp-rename');

exports.default = function() {
  return src('src/*.js')
    .pipe(babel())
    .pipe(src('vendor/*.js'))
    .pipe(dest('output/'))
    .pipe(uglify())
    .pipe(rename({ extname: '.min.js' }))
    .pipe(dest('output/'));
}
```

## 模式：流动 streaming、缓冲 buffered 和空 empty 模式

src() 可以工作在三种模式下：缓冲、流动和空模式，这些模式可以通过对 src() 的 buffer 和 read 参数进行设置

* 缓冲模式是默认模式，将文件内容加载内存中。插件通常运行在缓冲模式下，并且许多插件不支持流动模式
* 流动模式的存在主要用于操作无法放入内存的大文件
* 空模式不包含任何内容，仅在处理文件元数据时有用
