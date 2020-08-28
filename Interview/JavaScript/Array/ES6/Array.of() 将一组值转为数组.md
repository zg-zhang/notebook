# Array.of() 将一组值转为数组

Array.of 方法用于将一组值，转换为数组
```javascript
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1
```
这个方法的主要目的是弥补构造函数 Array()  的不足，因为参数个数的不同，会导致 Array() 的行为有差异
```javascript
Array() // []
Array(3) // [, , ,]
Array(3, 11, 8) // [3, 11, 8]
```
Array.of 基本上可以用来替代 Array() 或 new Array()，并且不存在由于参数不同而导致的重载，它的行为非常统一

Array.of 总是返回参数值组成的数组。如果没有参数，就返回一个空数组。


Array.of 方法可以用下面的代码模拟实现
```javascript
function ArrayOf(){
  return [].slice.call(arguments);
}
```


