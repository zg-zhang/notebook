# 处理请求 body 数据

## 需求分析

我们通过 XMLHttpRequest 对象实例的 send 方法来发送请求，并通过该方法的参数设置请求 body 数据

我们发现 send 方法的参数支持 Document 和 BodyInit 类型，当没有数据的时候，我们还可以传入 null

```typescript
axios({
  method: 'post',
  url: '/base/post',
  data: { 
    a: 1,
    b: 2 
  }
})
```

但是，这个时候 data 是不能直接传给 send 函数的，我们需要把它转换成 JSON 字符串

## transformRequest 函数实现

### data.ts

```typescript
import { isPlainObject } from './util'

export function transformRequest(data: any): any {
  if (isPlainObject(data)) {
    return JSON.stringify(data)
  }
  return data
}
```

### util.ts

```typescript
export function isPlainObject(val: any): val is Object {
  return toString.call(val) === '[object Object]'
}
```

### url.ts

```typescript
if (isDate(val)) {
  val = val.toISOString()
} else if (isPlainObject(val)) {
  val = JSON.stringify(val)
}
```
