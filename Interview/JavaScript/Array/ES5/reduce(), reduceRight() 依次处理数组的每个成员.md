# reduce(), reduceRight() 依次处理数组的每个成员

reduce 方法和 reduceRight 方法依次处理数组的每个成员，最终累计为一个值。它们的差别是：

- reduce 是从左到右处理，从第一个成员到最后一个成员
- reduceRight 是从右到左处理

其他完全一样
```javascript
[1, 2, 3, 4, 5].reduce(function (a, b) {
  console.log(a, b);
  return a + b;
})
// 1 2
// 3 3
// 6 4
// 10 5
//最后结果：15
```
reduce 和 reduceRight 方法的第一个参数都是一个函数，该函数接受以下四个参数：前两个为必选

1. 累计变量，默认为数组的第一个成员
1. 当前变量，默认为数组的第二个成员
1. 当前位置，从0开始
1. 原数组



如果要对累积变量指定初值，可以把它放在 reduce 方法和 reduceRight 方法的第二个参数
```javascript
// 注意，这里 a 的初始值为 10，b 是从数组的第一个成员开始便利
[1, 2, 3, 4, 5].reduce(function (a, b) {
  return a + b;
}, 10);
// 25
```
上面的第二个参数相当于设定了默认值，处理空数组时尤其有用
```javascript
function add(prev, cur) {
  return prev + cur;
}

[].reduce(add)
// TypeError: Reduce of empty array with no initial value
[].reduce(add, 1)
// 1
```
由于这两个方法会遍历数组，所以实际上还可以用来做一些遍历相关的操作。比如，找出字符串长度最长的数组成员
```javascript
function findLongest(entries) {
  return entries.reduce(function (longest, entry) {
    return entry.length > longest.length ? entry : longest;
  }, '');
}

findLongest(['aaa', 'bb', 'c']) // "aaa"
```
