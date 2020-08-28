# includes() 数组是否包含某个值

includes 方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的 includes 方法类似
```javascript
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true
```
## 第二参数
该方法的第二个参数表示搜索的起始位置，默认为 0，如果第二个参数为负数，则表示倒数的位置，如果它这时大于数组长度（比如第二个参数为`-4`，但数组长度为`3`），则会重置为从 0 开始
```javascript
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
```
## 有关 indexOf()
没有该方法之前，我们通常使用数组的 indexOf() 方法，检查是否包含某个值
```javascript
if (arr.indexOf(el) !== -1) {
  // ...
}
```
indexOf 方法内部使用 === 进行判断，这会导致对 NaN 的误判
includes 使用的是不一样的判断算法，就没有这个问题
```javascript
[NaN].indexOf(NaN)
// -1

[NaN].includes(NaN)
// true
```
## 如果不支持
```javascript
const contains = (() =>
  Array.prototype.includes
    ? (arr, value) => arr.includes(value)
    : (arr, value) => arr.some(el => el === value)
)();
contains(['foo', 'bar'], 'baz'); // => false
```
## 和 has 方法的区分
另外，Map 和 Set 数据结构有一个`has`方法，需要注意与`includes`区分。

- Map 结构的`has`方法，是用来查找键名的，比如`Map.prototype.has(key)`、`WeakMap.prototype.has(key)`、`Reflect.has(target, propertyKey)`。
- Set 结构的`has`方法，是用来查找值的，比如`Set.prototype.has(value)`、`WeakSet.prototype.has(value)`。
