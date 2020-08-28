# Array.from() 对象转数组

Array.from 方法用于将两类对象转为真正的数组：

- 类似数组的对象 array-like object
- 可便利的对象 iterable 包括 ES6 新增的数据结构 Set 和 Map
```javascript
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ES5的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']

// ES6的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```
实际使用中，常见的类似数组的对象是 DOM 操作返回的 NodeList 集合，以及函数内部的 arguments 对象。Array.from 都可以将它们转为真正的数组。
```javascript
// NodeList对象
let ps = document.querySelectorAll('p');
Array.from(ps).filter(p => {
  return p.textContent.length > 100;
});

// arguments对象
function foo() {
  var args = Array.from(arguments);
  // ...
}
```
只要是部署了 Iterator 接口的数据结构，Array.from 都能将其转为数组。
如果参数是一个真正的数组，Array.from 会返回一个一模一样的新数组。

扩展运算符背后调用的是遍历器接口 Symbol.iterator，如果一个对象没有部署这个接口，就无法转换。Array.from 方法还支持类似数组的对象。所谓类似数组的对象，本质特征只有一点，必须有 length 属性。因此，任何有 length 属性的对象，都可以通过 Array.from 方法转为数组，而此时扩展运算符就无法转换
```javascript
Array.from({ length: 3 });
// [ undefined, undefined, undefined ]
```
对于还没有部署该方法的浏览器，可以用 Array.prototype.slice 方法替代
```javascript
const toArray = (() =>
  Array.from ? Array.from : obj => [].slice.call(obj)
)();
```
## 第二个参数
Array.from 方法还可以接受第二个参数，作用类似于数组的 map 方法，用来对每个元素进行处理，将处理后的值放入返回的数组
```javascript
Array.from(arrayLike, x => x * x);
// 等同于
Array.from(arrayLike).map(x => x * x);

Array.from([1, 2, 3], (x) => x * x)
// [1, 4, 9]
```
下面是一些例子：
```javascript
let spans = document.querySelectorAll('span.name');

// map()
let names1 = Array.prototype.map.call(spans, s => s.textContent);

// Array.from()
let names2 = Array.from(spans, s => s.textContent)

Array.from([1, , 2, , 3], (n) => n || 0)
// [1, 0, 2, 0, 3]
```
## 第三个参数
如果 map 函数里面用到了 this 关键字，还可以传入第三个参数，用来绑定 this

Array.from 方法可以将各种值转为真正的数组，并且还提供 map 功能，这实际上意味着，只要有一个原始的数据结构，你就可以先对它的值进行处理，然后转成规范的数组结构，进而就可以使用数量众多的数组方法
```javascript
// 第一个参数指定了第二个参数运行的次数，这种特性可以让该方法的用法变得非常灵活
Array.from({ length: 2 }, () => 'jack')
// ['jack', 'jack']
```
Array.from 方法的另一个应用是，将字符串转为数组，然后返回字符串的长度。因为它能正确处理各种 Unicode 字符，可以避免 JavaScript 将大于`\uFFFF`的 Unicode 字符，算作两个字符的 bug。
```javascript
function countSymbols(string) {
  return Array.from(string).length;
}
```
