# shift(), unshift() [队列]

## shift()
shift 方法用于删除数组的第一个元素，并返回该元素。注意，该方法会改变原数组
```javascript
var a = ['a', 'b', 'c'];

a.shift() // 'a'
a // ['b', 'c']
```
shift 方法可以遍历并清空一个数组，前提是数组元素不能是 0 或者任何布尔值等于 false 的元素，因此这样的遍历是不可靠的
```javascript
var list = [1, 2, 3, 4];
var item;

while (item = list.shift()) {
  console.log(item);
}

list // []
```
push 和 shift 的结合使用，就构成了 **先进先出** 的队列结构 queue
## unshift()
unshift 方法用于在数组的第一个位置添加元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。
```javascript
var a = ['a', 'b', 'c'];

a.unshift('x'); // 4
a // ['x', 'a', 'b', 'c']
```
unshift 方法可以接受多个参数，这些参数都会添加到目标数组的头部
```javascript
var arr = [ 'c', 'd' ];
arr.unshift('a', 'b') // 4
arr // [ 'a', 'b', 'c', 'd' ]
```


