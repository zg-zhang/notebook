# some(), every() [断言] 是否符合条件

这两个方法类似断言。返回一个布尔值，表示判断数组成员是否符合某种条件
它们接受一个函数作为参数，所有数组成员依次执行该函数。该函数接受三个参数：当前成员、当前位置和整个数组，然后返回一个布尔值


注意，对于空数组，some 方法返回 false， every 返回 true，回调函数都不会执行

some 和 every 方法还可以接受第二个参数，用来绑定参数函数内部的 this 变量
## some()
some 方法是只要一个成员的返回值是 true，则整个 some 方法的返回值就是 true，否则返回 false
```javascript
var arr = [1, 2, 3, 4, 5];
arr.some(function (elem, index, arr) {
  return elem >= 3;
});
// true
```
## every()
every 方法是所有成员的返回值都是 true，整个 every 方法才返回 true，否则返回 false
```javascript
var arr = [1, 2, 3, 4, 5];
arr.every(function (elem, index, arr) {
  return elem >= 3;
});
// false
```


