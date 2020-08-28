# find(), findIndex() 找出符合条件的成员

## find()
find 方法，用于找出第一个复合条件的数组成员，它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为 true 的成员，然后返回该成员。


如果没有符合条件的成员，则返回 undefined
```javascript
[1, 4, -5, 10].find((n) => n < 0)
// -5
```
find 方法的回调函数可以接受三个参数：当前的值，当前的位置，原数组
```javascript
[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10
```
## findIndex()
findIndex 方法的用法和 find 方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回 -1
```javascript
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```
## 第二个参数
这两个方法都可以接受第二个参数，用来绑定回调函数的 this 对象
```javascript
function f(v){
  return v > this.age;
}
let person = {name: 'John', age: 20};
[10, 12, 26, 15].find(f, person);    // 26
```
## 发现NaN
这两个方法都可以发现 NaN，弥补了数组的 indexOf 方法的不足


indexOf 方法无法识别数组的 NaN 成员，但是 findIndex 方法可以借助 Object.is 方法做到
```javascript
[NaN].indexOf(NaN)
// -1

[NaN].findIndex(y => Object.is(NaN, y))
// 0
```


