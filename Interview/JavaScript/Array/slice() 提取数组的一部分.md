# slice() 提取数组的一部分

slice 方法用于提取目标数组的一部分，返回一个新数组，原数组不变
```javascript
arr.slice(start, end);
```
它的第一个参数为起始位置（从 0 开始，会包括在返回的新数组之中），第二个参数为终止位置（但该位置的元素本身不包括在内）。如果省略第二个参数，则一直返回到原数组的最后一个成员。
```javascript
var a = ['a', 'b', 'c'];

a.slice(0) // ["a", "b", "c"]
a.slice(1) // ["b", "c"]
a.slice(1, 2) // ["b"]
a.slice(2, 6) // ["c"]
a.slice() // ["a", "b", "c"]
```
如果 slice 方法的参数是负数，则表示倒数计算的位置，-1 表示倒数计算的第一个位置
```javascript
var a = ['a', 'b', 'c'];
a.slice(-2) // ["b", "c"]
a.slice(-2, -1) // ["b"]
```
如果第一个参数大于等于数组长度，或者第二个参数小于第一个参数，则返回空数组
```javascript
var a = ['a', 'b', 'c'];
a.slice(4) // []
a.slice(2, 1) // []
```
slice 方法的一个重要应用，是将类似数组的对象转为真正的数组。下面的都不是数组，但是可以通过 call 方法，在它们上面调用 slice 方法，将它们转为真正的数组
```javascript
Array.prototype.slice.call({ 0: 'a', 1: 'b', length: 2 })
// ['a', 'b']

Array.prototype.slice.call(document.querySelectorAll("div"));
Array.prototype.slice.call(arguments);
```


