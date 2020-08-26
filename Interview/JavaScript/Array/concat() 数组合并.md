# concat() 数组合并

concat 方法用于多个数组的合并，它将新数组的成员，添加到原数组成员的后部，然后返回一个新数组，原数组不变。
```javascript
['hello'].concat(['world'])
// ["hello", "world"]

['hello'].concat(['world'], ['!'])
// ["hello", "world", "!"]

[].concat({a: 1}, {b: 2})
// [{ a: 1 }, { b: 2 }]

[2].concat({a: 1})
// [2, {a: 1}]
```
除了数组参数，concat 也接受其他类型的值作为参数，添加到目标数组尾部
```javascript
[1, 2, 3].concat(4, 5, 6)
// [1, 2, 3, 4, 5, 6]
```
如果数组成员包括对象，concat 方法返回当前数组的一个浅拷贝。所谓 浅拷贝，指的是新数组拷贝的是对象的引用


下面的代码中，原数组包含一个对象，concat 方法生成的新数组包含这个对象的引用。所以，改变原对象后，新数组跟着改变。
```javascript
var obj = { a: 1 };
var oldArray = [obj];

var newArray = oldArray.concat();

obj.a = 2;
newArray[0].a // 2
```
