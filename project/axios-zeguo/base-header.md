# 处理请求 header

上一节里，虽然我们已经将 data 转换成了 JSON 字符串，但是数据发送到服务端的时候，服务端并不能正常解析我们发送的数据

因为我们并没有给请求 header 设置正确的 Content-Type

```typescript
axios({
  method: 'post',
  url: '/base/post',
  headers: {
    'content-type': 'application/json;charset=utf-8'
  },
  data: {
    a: 1,
    b: 2
  }
})
```

## processHeaders 函数实现

### headers.ts

```typescript
import { isPlainObject } from './util'

// 属性名规范化 因为 header 属性是大小写不敏感的
function normalizeHeaderName(headers: any, normalizedName: string): void {
  // 如果没有 headers 这个属性就直接退出
  if (!headers) {
    return
  }
  Object.keys(headers).forEach(name => {
    // name 和 normalizedName 不同但是他们的大写一样
    if (name !== normalizedName && name.toUpperCase() === normalizedName.toUpperCase()) {
      headers[normalizedName] = headers[name]
      // delete 操作符，用于删除对象的某个属性，有返回值是布尔值
      delete headers[name]
    }
  })
}

export function processHeaders(headers: any, data: any): any {
  normalizeHeaderName(headers, 'Content-Type')

  if (isPlainObject(data)) {
    if (headers && !headers['Content-Type']) {
      headers['Content-Type'] = 'application/json;charset=utf-8'
    }
  }
  return headers
}
```

## 实现请求 header 处理逻辑

因为我们处理 header 的时候依赖 data 所以要在处理请求 body 数据之前处理请求 header 

```typescript
export default function xhr(config: AxiosRequestConfig): void {
  const { data = null, url, method = 'get', headers } = config

  const request = new XMLHttpRequest()

  request.open(method.toUpperCase(), url, true)

  Object.keys(headers).forEach(name => {
    // 当传入的 data 为空的时候，配置 content-type 是没有意义的，所以我们把他删除
    if (data === null && name.toLowerCase() === 'content-type') {
      delete headers[name]
    } else {
      request.setRequestHeader(name, headers[name])
    }
  })

  request.send(data)
}
```
