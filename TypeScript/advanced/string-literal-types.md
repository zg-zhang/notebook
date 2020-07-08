# 字符串字面量类型

字符串字面量类型用来约束取值只能是某个字符串中的一个

## 简单例子：

```typescript
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}

handleEvent(document.getElementById('hello'), 'scroll'); // true
handleEvent(document.getElementById('world'), 'dblclick'); // error 不能为 dblclick
```

上面 我们使用 type 定了一个字符串字面量类型 EventNames，它只能取三种字符串中的一种

注意，类型别名与字符串字面量类型都是使用 type 进行定义
