# Android 上的 WebSockets 变得简单

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/websockets-on-android-made-easy>

 在我们的内部项目中使用 WebSockets 一段时间后，我想与你分享我的想法和经验。

## 有什么问题？

在我们的应用中，我们需要一种设备间本地通信的解决方案。在寻找解决方案时，我们不得不考虑其中一台设备需要作为服务器使用。经过快速研究，我们发现最佳选择是 Java WebSockets 库。主要优势是它能够在 android 设备上同时处理服务器和客户端。这些功能的主要实现需要扩展两个类: *WebSocketClient* 和 *WebSocketServer* 、 并覆盖它们的函数。剩下的逻辑来自正确的消息处理。

## WebSocket 服务器

为了实现 sockets 服务器，我们需要实现基本的钩子函数。

```
import org.java_websocket.WebSocket
import org.java_websocket.handshake.ClientHandshake
import org.java_websocket.server.WebSocketServer
import java.net.InetSocketAddress

class CustomWebSocketServer(
   port: Int? = null
) : WebSocketServer(InetSocketAddress(port ?: PORT)) {

   override fun onOpen(conn: WebSocket?, handshake: ClientHandshake?) = Unit

   override fun onClose(conn: WebSocket?, code: Int, reason: String?, remote: Boolean) = Unit

   override fun onMessage(conn: WebSocket?, message: String?) = Unit

   override fun onMessage(conn: WebSocket?, message: ByteBuffer?) = Unit

   override fun onStart() = Unit

   override fun onError(conn: WebSocket?, ex: Exception?) = Unit

   companion object {
      internal const val PORT = 50123
   }
} 
```

因为我们正在处理一个跨平台的应用程序，我们还需要覆盖 onMessage(conn: WebSocket？，消息:ByteBuffer？) 因为 iOS 是以字节为单位发送消息的。这就是我们也以字节形式发送消息的原因。如果我们需要保持设备连接到我们的服务器，每一个设备都要经过 onOpen ，只需保持对conn:web socket的引用。我们的项目规范要求我们的服务器同时与所有客户端通信，因此我们不需要特定的引用。服务器能够发送广播，所以它会派上用场。

在我们开始实现消息处理之前，请记住，如果您想继续重用同一个端口，您应该添加

```
isReuseAddr = true
```

在您的 init 块中。如果您不添加它，您要么必须在每次启动时更改您的端口，要么您的服务器有时会在同一个端口上启动时出现问题。

要启动服务器，你必须调用 start() 方法。这个方法会阻塞主线程。在我们的项目中，我们使用 RxJava 在后台执行它。

说到消息传递，正如我前面提到的，为了支持 iOS，我们还需要覆盖 second onMessage 函数:

```
override fun onMessage(conn: WebSocket?, message: ByteBuffer?) {
   message ?: return
   val buffer = ByteArray(message.remaining())
   message.get(buffer)
   onMessage(conn, String(buffer, Charset.defaultCharset()))
} 
```

现在，我们也能够接收来自 iOS 的消息，我们可以实际与消息进行交互。那么，让我们在构造函数中添加一个高阶函数:

```
class CustomWebSocketServer(
   port: Int? = null,
   private val onMessageReceived: (WebSocket?, String?) -> Unit
) : WebSocketServer(InetSocketAddress(port ?: PORT)) {
…
   override fun onMessage(conn: WebSocket?, message: String?) {
      onMessageReceived(conn, message)
   }
…
} 
```

我们马上就会回到我们的服务器。是时候和客户一起做些工作了。

## WebSocket 客户端

```
import org.java_websocket.client.WebSocketClient
import org.java_websocket.handshake.ServerHandshake
import java.net.URI

class CustomWebSocketClient(
   val address: String
) : WebSocketClient(URI(address)) {

   override fun onOpen(handshakedata: ServerHandshake?)  = Unit

   override fun onClose(code: Int, reason: String?, remote: Boolean)  = Unit

   override fun onMessage(message: String?)  = Unit

   override fun onError(ex: Exception?)  = Unit
}
```

正如你在服务器超级构造器上看到的，我们需要做的就是用给定的端口传递InetSocketAddress在客户端我们必须用给定的模式" ws://$ { ServerIP }:$ { WebSocketPort } "传递 URI 。从这里你所要做的就是从后台线程调用 connect() ，因为就像服务器一样，它会阻塞主线程。我们需要覆盖 onMessage(bytes: ByteBuffer？)从 iOS 服务器接收信息。

```
class CustomWebSocketClient(
   address: String,
   private val onMessageReceived: (String?) -> Unit
) : WebSocketClient(URI(address)) {
…
   override fun onMessage(message: String?) {
      onMessageReceived(message)
   }

   override fun onMessage(bytes: ByteBuffer?) {
      bytes ?: return
      val buffer = ByteArray(bytes.remaining())
      bytes.get(buffer)
      onMessage(String(buffer, Charset.defaultCharset()))
   }
…
} 
```

现在我们已经实现了客户端和服务器，终于到了发送一些消息的时候了。

## 信息发送

我们决定使用 JSON 格式处理消息。为了构造和管理我们的消息，我们可以使用 org.json 包中的 JSONObject 类型。

如果我们想要从我们的服务器广播特定的操作来通知所有连接的客户端，我们所要做的就是创建一个简单的消息并如下发送它:

```
val jsonObject = JSONObject().apply {
   put("notify", "remember to eat your vegetables")
}

server?.broadcast(jsonObject.toString().toByteArray(Charset.defaultCharset())) 
```

在客户端，我们现在可以显示收到的通知:

```
CustomWebSocketClient(
   address =  "ws://192.160.0.98:50123",
   onMessageReceived = { message ->
      val jsonObject = JSONObject(message)
      if (jsonObject.has("notify")) {
         showSnackbar(jsonObject.getString("notify"))
      }
   }
) 
```

这就是实现简单的基于套接字的服务器-客户端通信所需的全部内容。现在，您可以使用一些额外的功能来扩展您的实现，比如通知连接性的变化或添加客户端-服务器消息传递。

作为专业建议，我想提一下 Java WebSockets 最大的缺点是，当你的服务器崩溃时，你需要创建一个新的实例。幸运的是，在客户端，您可以只调用 reconnect()

## 最终世界

当您的应用程序需要维护客户端应用程序和服务器之间的持久连接时，WebSockets 就派上了用场。至于我们的需求，我们设法处理音乐播放中的变化，以及简单地通知客户端应用程序某些事件。如果您对构建客户端-服务器应用程序感兴趣，请告诉我们。我们很乐意帮忙。

* * *

照片由 rawpixel 上 [下](http://web.archive.org/web/20221201143122/https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)