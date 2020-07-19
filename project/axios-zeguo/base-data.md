# 获取响应数据

## 需求分析

在前面的章节中，我们发送的请求都可以从网络层面接收到服务端返回的数据，但是代码层面并没有做任何关于返回数据的处理

我们可以拿到 res 对象，并且我们希望该对象包括：
* 服务端返回的数据 data
* HTTP 状态码 status
* 状态消息 statusText
* 响应头 headers
* 请求配置对象 config
* 请求 XMLHttpRequest 对象实例 request

## 定义接口类型

