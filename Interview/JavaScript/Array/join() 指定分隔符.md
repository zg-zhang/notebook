# join() 指定分隔符

join 方法以指定参数作为分隔符，将所有数组成员连接为一个字符串返回，如果不提供参数，默认用逗号分隔
```javascript
var a = [1, 2, 3, 4];

a.join(' ') // '1 2 3 4'
a.join(' | ') // "1 | 2 | 3 | 4"
a.join() // "1,2,3,4"
```
如果数组成员是 undefined 或 null 或 空位，会被转成空字符串
```javascript
[undefined, null].join('#')
// '#'

['a',, 'b'].join('-')
// 'a--b'
```
通过 call 方法，这个方法也可以用于字符串或类似数组的对象
```javascript
Array，prototype.join.call('hello', '-')
// "h-e-l-l-o"

var obj = { 0: 'a', 1: 'b', length: 2 };
Array.prototype.join.call(obj, '-')
// 'a-b'
```


