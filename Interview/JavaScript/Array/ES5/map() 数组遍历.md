# map() 数组遍历

map方法将数组的所有成员依次传入参数函数，然后把每一次的执行结果组成一个新数组返回
下面的代码中，numbers 数组的所有成员依次执行参数函数，运行结果组成一个新数组返回，原数组没有变化
```javascript
var numbers = [1, 2, 3];

numbers.map(function (n) {
  return n + 1;
});
// [2, 3, 4]

numbers
// [1, 2, 3]
```
map 方法接受一个函数作为参数，该函数调用时，map 方法向它传入三个参数：当前成员、当前位置和数组本身
```javascript
[1, 2, 3].map(function(elem, index, arr) {
  return elem * index;
});
// [0, 2, 6]
```
map 方法还可以接受第二个参数，用来绑定回调函数内部的 this 变量
下面代码通过 map 方法的第二个参数，将回调函数内部的 this 对象，指向 arr 数组
```javascript
var arr = ['a', 'b', 'c'];

[1, 2].map(function (e) {
  return this[e];
}, arr)
// ['b', 'c']
```
如果数组有空位，map 方法的回调函数在这个位置不会执行，会跳过数组的空位
map 不会跳过 undefined 和 null
```javascript
var f = function (n) { return 'a' };

[1, undefined, 2].map(f) // ["a", "a", "a"]
[1, null, 2].map(f) // ["a", "a", "a"]
[1, , 2].map(f) // ["a", , "a"]
```
