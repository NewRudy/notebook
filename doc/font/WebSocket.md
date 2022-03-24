# WebSocket

[toc]

[知乎](https://zhuanlan.zhihu.com/p/74326818)

[教程](http://www.ruanyifeng.com/blog/2017/05/websocket.html)

## 概念

> WebSocket是一种通信协议，可在单个TCP连接上进行全双工通信。WebSocket使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在WebSocket API中，浏览器和服务器只需要完成一次握手，两者之间就可以建立持久性的连接，并进行双向数据传输。

- WebSocket.onopen： 连接成功后的回调
- WebSocket.onclose： 连接关闭后的回调
- WebSocket.onerror： 连接失败后的回调
- WebSocket.onmessage： 客户端接收到服务端数据的回调
- webSocket.bufferedAmount： 未发送至服务器的二进制字节数
- WebSocket.binaryType： 使用二进制的数据类型连接
- WebSocket.protocol ： 服务器选择的下属协议
- WebSocket.url ： WebSocket 的绝对路径
- WebSocket.readyState： 当前连接状态，对应的四个常量

WebSocket.CONNECTING: 0

WebSocket.OPEN: 1

WebSocket.CLOSING: 2

WebSocket.CLOSED: 3

方法：

- WebSocket.close() 关闭当前连接
- WebSocket.send(data) 向服务器发送数据



## Frp

[gitHub](https://github.com/fatedier/frp)

[doc](https://gofrp.org/docs/)

frp是一种快速反向代理，可帮助您将NAT或防火墙后面的本地服务器公开到Internet。到目前为止，它支持**TCP**和**UDP**以及**HTTP**和**HTTPS**协议，在这些协议中，请求可以通过域名转发到内部服务。

