# indexOf(), lastIndexOf() 出现的位置

注意，这两个方法不能用来搜索 NaN 的位置，即它们无法确定数组成员是否包含 NaN
因为，这两个方法内部使用的是 === ，而 NaN 是唯一一个不等于自身的值
## indexOf()
indexOf 方法返回给定元素在数组中第一次出现的位置，如果没有出现则返回 -1
```javascript
var a = ['a', 'b', 'c'];

a.indexOf('b') // 1
a.indexOf('y') // -1
```
indexOf 方法还可以接受第二个参数，表示搜索的开始位置
```javascript
['a', 'b', 'c'].indexOf('a', 1) // -1
```
## lastIndexOf()
lastIndexOf 方法返回给定元素的数组中最后一次出现的位置，如果没有出现则返回 -1
```javascript
var a = [2, 5, 9, 2];
a.lastIndexOf(2) // 3
a.lastIndexOf(7) // -1
```


