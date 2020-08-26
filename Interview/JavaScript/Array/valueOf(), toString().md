# valueOf(), toString()

## valueOf()
valueOf 方法是一个所有对象都拥有的方法，表示对该对象求值。不同对象的 valueOf 方法不尽相同，数组的 valueOf 方法返回数组本身
```javascript
var arr = [1, 2, 3];
arr.valueOf() // [1, 2, 3]
```
## toString()
toString 方法也是对象的通用方法，数组的 toString 方法返回数组的字符串形式
```javascript
var arr = [1, 2, 3];
arr.toString() // "1,2,3"

var arr = [1, 2, 3, [4, 5, 6]];
arr.toString() // "1,2,3,4,5,6"
```


