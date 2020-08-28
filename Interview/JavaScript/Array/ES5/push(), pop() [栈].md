# push(), pop() [栈]

## push()
push 方法用于在数组的末端添加一个或多个元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。
```javascript
var arr = [];

arr.push(1) // 1
arr.push('a') // 2
arr.push(true, {}) // 4
arr // [1, 'a', true, {}]
```
## pop()
pop 方法用于删除数组的最后一个元素，并返回该元素。注意，该方法会改变原数组
```javascript
var arr = ['a', 'b', 'c'];

arr.pop() // 'c'
arr // ['a', 'b']
```
对空数组使用 pop 方法，不会报错，而是返回 undefined
```javascript
[].pop() // undefined
```
push 和 pop 结合使用，就构成了 **后进先出** 的 栈结构 stack
