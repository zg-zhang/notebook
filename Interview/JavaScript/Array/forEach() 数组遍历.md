# forEach() 数组遍历

forEach 方法和 map 方法很相似，也是对数组的所有成员依次执行参数函数。但是 forEach 方法不返回值，只用来操作数据。
如果数组遍历的目的是为了得到返回值，那么使用 map 方法，否则使用 forEach 方法
forEach 的用法和 map 方法一致
```javascript
function log(element, index, array) {
  console.log('[' + index + '] = ' + element);
}

[2, 5, 9].forEach(log);
// [0] = 2
// [1] = 5
// [2] = 9
```
forEach 方法也可以接受第二个参数，绑定参数函数的 this 变量
```javascript
var out = [];

[1, 2, 3].forEach(function(elem) {
  this.push(elem * elem);
}, out);

out // [1, 4, 9]
```
注意，forEach 方法无法中断执行，总是会将所有成员遍历完。如果希望符合某种条件时，就中断遍历，要使用 for 循环
forEach 方法也会跳过数组的空位，但不会跳过 undefined 和 null
```javascript
var log = function (n) {
  console.log(n + 1);
};

[1, undefined, 2].forEach(log)
// 2
// NaN
// 3

[1, null, 2].forEach(log)
// 2
// 1
// 3

[1, , 2].forEach(log)
// 2
// 3
```
