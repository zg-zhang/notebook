# entries(), key(), values() 遍历数组

ES6 提供了三个新的方法：enties(), key(), values() 用于遍历数组。
它们都返回一个遍历器对象，可以用 for ... of 循环进行遍历，唯一的区别是：

- keys() 是对键名的遍历
- values() 是对键值的遍历
- entries() 是对键值对的遍历
```javascript
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```
如果不适用 for ... of ，可以手动调用遍历器对象的 next 方法，进行遍历
```javascript
let letter = ['a', 'b', 'c'];
let entries = letter.entries();
console.log(entries.next().value); // [0, 'a']
console.log(entries.next().value); // [1, 'b']
console.log(entries.next().value); // [2, 'c']
```


