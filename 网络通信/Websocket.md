# WebSocket

## 消息推送常见方式

轮询、长轮询、SSE、websocket



轮询：

- 浏览器以指定的时间间隔向服务器发出HTTP请求，服务器实时返回数据给浏览器。

长轮询：

- 浏览器发出ajax请求，服务器端接收到请求后，会阻塞请求指导有数据或者超时才返回。

SSE：

- (server-sent-event )服务器发送事件

- SSE在服务器和客户端之间打开一个单向通道
- 服务端响应的不再是一次性的数据包，而是text/event-stream类型的数据流信息
- 服务器有数据变更时数据流式传输到客户端



## websocket介绍

websocket是一种基于TCP连接上进行全双工通信的协议

全双工：允许数据在两个方向上同时传输

半双工：允许数据在两个方向上传输，但是同一个时间段内只允许一个方向上传输。

传统的 HTTP 协议是**请求-响应模式**：客户端发起请求，服务器响应后连接就关闭。而 WebSocket 允许**服务器主动向客户端推送数据**，并且连接可以持续保持。



## WebSocket 与 HTTP 的区别

| 特性           | HTTP                     | WebSocket          |
| -------------- | ------------------------ | ------------------ |
| 通信模式       | 客户端请求，服务器响应   | 双向通信           |
| 连接方式       | 每次请求都建立新连接     | 一次连接，持续存在 |
| 延迟           | 高（每次都需要建立连接） | 低（连接复用）     |
| 传输效率       | 低（请求头大，频繁连接） | 高（帧结构轻量）   |
| 服务器主动推送 | ❌ 不支持                 | ✅ 支持             |



## WebSocket 工作流程

1. 发起握手请求（HTTP Upgrade）

   客户端使用 HTTP 协议发起一次特殊的请求，要求升级到 WebSocket 协议。

   ```http
   GET /chat HTTP/1.1
   Host: example.com
   Upgrade: websocket
   Connection: Upgrade
   Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
   Sec-WebSocket-Version: 13
   ```

2. 服务器响应

   如果服务器支持 WebSocket，会返回 101 Switching Protocols 状态码。

   ```http
   HTTP/1.1 101 Switching Protocols
   Upgrade: websocket
   Connection: Upgrade
   Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
   ```

3. 建立连接
    握手成功后，客户端和服务器之间建立了一个持续的 WebSocket 连接，可以互相发送消息。

4. 双向通信
    任一方都可以随时发送数据，格式为 WebSocket 帧（frame）。

5. 关闭连接
    任一方可以发起关闭请求，断开连接。



## websocket API

### websocket对象创建

#### 客户端

```javascript
const socket = new WebSocket("ws://localhost:8080");

socket.onopen = () => {
  console.log("连接已建立");
  socket.send("你好，服务器！");
};

socket.onmessage = (event) => {
  console.log("收到消息:", event.data);
};

socket.onclose = () => {
  console.log("连接已关闭");
};
```

##### websocket事件

| 事件        | 说明         | 示例代码                       |
| ----------- | ------------ | ------------------------------ |
| `onopen`    | 连接建立成功 | `socket.onopen = () => {}`     |
| `onmessage` | 接收到消息   | `socket.onmessage = (e) => {}` |
| `onerror`   | 出现错误     | `socket.onerror = (e) => {}`   |
| `onclose`   | 连接关闭     | `socket.onclose = (e) => {}`   |

##### WebSocket 的方法

| 方法                    | 说明                           |
| ----------------------- | ------------------------------ |
| `send(data)`            | 发送字符串或二进制数据到服务器 |
| `close([code, reason])` | 关闭连接，可传错误码和原因     |







#### 服务端

```java
import javax.websocket.*;
import javax.websocket.server.ServerEndpoint;
import java.io.IOException;

@ServerEndpoint("/chat")
public class ChatEndpoint {

    @OnOpen
    public void onOpen(Session session) throws IOException {
        System.out.println("连接开启: " + session.getId());
        session.getBasicRemote().sendText("欢迎连接 WebSocket！");
    }

    @OnMessage
    public void onMessage(String message, Session session) throws IOException {
        System.out.println("收到消息: " + message);
        session.getBasicRemote().sendText("你发送了: " + message);
    }

    @OnClose
    public void onClose(Session session) {
        System.out.println("连接关闭: " + session.getId());
    }

    @OnError
    public void onError(Session session, Throwable throwable) {
        System.err.println("发生错误: " + throwable.getMessage());
    }
}
```













































