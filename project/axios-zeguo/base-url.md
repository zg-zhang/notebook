# 处理请求 url 参数

## url 分析

### key 和 value

```typescript
axios({
    method: 'get',
    url: '/base/get',
    params: {
        a: 1,
        b: 2
    }
})
```

目标url: `/base/get?a=1&b=2`

### 数组

```typescript
axios({
    method: 'get',
    url: '/base/get',
    params: {
        foo: ['bar', 'baz']
    }
})
```

目标url: `/base/get?foo[]=bar&foo[]=baz`

### 对象

```typescript
axios({
    method: 'get',
    url: '/base/get',
    params: {
        foo: {
            bar: 'baz'
        }
    }
})
```

目标url: `/base/get?foo=%7B%22bar%22:%22baz%22%7D`

后面拼接的是 `{"bar": "baz"}` encode 后的结果

### Date 类型

```typescript
const date = new Date();

axios({
    method: 'get',
    url: '/base/get',
    params: {
        date
    }
})
```

目标url: `/base/get?date=2019-04-01T05:55:39.030Z`

后面拼接的是 date.toISOString()

### 特殊字符支持

对于字符 `@`、`:`、`$`、`,`、` `、`[`、`]`，我们是允许出现在 url 中的，不希望被 encode

```typescript
axios({
    method: 'get',
    url: '/base/get',
    params: {
        foo: '@:$, '
    }
})
```

目标url: `/base/get?foo=@:$,+`

注意，我们会把空格转换成 + 

### 空值忽略

对于值为 null 或者 undefined 的属性，我们是不会添加到 url 参数中

```typescript
axios({
    method: 'get',
    url: '/base/get',
    params: {
        foo: 'bar',
        baz: null
    }
})
```

目标url: `/base/get?foo=bar`

### 丢弃 url 中的哈希标记

```typescript
axios({
    method: 'get',
    url: '/base/get#hash',
    params: {
        foo: 'bar'
    }
})
```

目标url: `/base/get?foo=bar`

### 保留 url 中已存在的参数

```typescript
axios({
    method: 'get',
    url: '/base/get?foo=bar',
    params: {
        bar: 'baz'
    }
})
```

目标url: `/base/get?foo=bar&bar=baz`

## buildURL 函数实现

这是一个把 params 拼接到 url 上的工具函数

url.ts

```typescript
import { isDate, isObject } from './util'

// 把 encode 转换成 特殊字符 的工具函数
function encode(val: string): string {
  return encodeURIComponent(val)
    .replace(/%40/g, '@')
    .replace(/%3A/gi, ':')
    .replace(/%24/g, '$')
    .replace(/%2C/gi, ',')
    .replace(/%20/g, '+')
    .replace(/%5B/gi, '[')
    .replace(/%5D/gi, ']')
}

export function buildURL(url: string, params?: any) {
  // 要是没有 params 就直接返回 url
  if (!params) {
    return url
  }

  const parts: string[] = []

  // 通过 Object.keys 获取 params 对象的 key 值的数组
  Object.keys(params).forEach(key => {
    let val = params[key]
    // 如果是 null 或者 undefined 就忽略
    if (val === null || typeof val === 'undefined') {
      return
    }
    let values: string[]
    // 判断 value 是不是数组，如果是在 key 的后面加 []
    if (Array.isArray(val)) {
      values = val
      key += '[]'
    } else {
      values = [val]
    }
    values.forEach(val => {
      // 判断是不是 Date
      if (isDate(val)) {
        val = val.toISOString() // 使用 ISO 标准返回 Date 对象的字符串对象格式
      } else if (isObject(val)) {
        val = JSON.stringify(val)
      }
      parts.push(`${encode(key)}=${encode(val)}`)
    })
  })

  let serializedParams = parts.join('&')

  if (serializedParams) {
    // 如果 url 中有 # 则挑出 baseUrl
    const markIndex = url.indexOf('#')
    if (markIndex !== -1) {
      url = url.slice(0, markIndex)
    }

    url += (url.indexOf('?') === -1 ? '?' : '&') + serializedParams
  }
    
  return url
}
```

util.ts

```typescript
const toString = Object.prototype.toString;

export function isDate(val: any): val is Date {
  return toString.call(val) === '[object Date]'
}

export function isObject(val: any): val is Object {
  return val !== null && typeof val === 'object'
}
```
