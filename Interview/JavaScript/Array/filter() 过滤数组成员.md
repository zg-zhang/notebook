# filter() 过滤数组成员

filter 方法用于过滤数组成员，满足条件的成员组成一个新数组返回
它的参数是一个函数，所有数组成员依次执行该函数，返回结果为 true 的成员组成一个新数组返回，该方法不会改变原数组
```javascript
[1, 2, 3, 4, 5].filter(function (elem) {
  return (elem > 3);
})
// [4, 5]

var arr = [0, 1, 'a', false];
arr.filter(Boolean)
// [1, "a"]
```
filter 方法的参数函数可以接受三个参数：当前成员，当前位置和整个数组
```javascript
[1, 2, 3, 4, 5].filter(function (elem, index, arr) {
  return index % 2 === 0;
});
// [1, 3, 5]
```
filter 方法还可以接受第二个参数，用来绑定参数函数内部的 this 变量
```javascript
var obj = { MAX: 3 };
var myFilter = function (item) {
  if (item > this.MAX) return true;
};

var arr = [2, 8, 3, 4, 1, 3, 2, 9];
arr.filter(myFilter, obj) // [8, 4, 9]
```


