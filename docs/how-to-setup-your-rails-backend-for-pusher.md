# 如何为 Pusher 设置 Rails 后端

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-to-setup-your-rails-backend-for-pusher>

 Pusher.js 准备使用 SaaS 提供多种技术的 websockets。无论有无授权，它都可以处理公共、私人和客户事件。

您将使用的内容:

[pusher gem](http://web.archive.org/web/20221202084845/https://github.com/pusher/pusher-http-ruby) -作为服务器 gem，负责向客户端触发事件(事件也可以在除服务器之外的客户端之间触发，但它需要更多的配置，这将在后面解决)。

### 优点:

*   易于设置，

### 提供的支持，

*   适用于 Ruby、iOS、Android 和 JavaScript。
*   客户名称:
*   步骤 1:设置初始化

### Pusher 将作为一个全局单例对象在初始化器中设置。您需要将 pusher.rb 添加到您的初始化器中，这将简单地定义应用凭证并启用加密。

### 步骤 2:创建 Pusher::Dispatcher 服务来触发事件

这一步取决于您选择的架构。我选择使用 Pusher::Dispatcher 类来触发 Pusher singleton。看起来有点过了，但是这是一个基于一些变量(例如用户 id)计算频道名称的好地方。

### 请注意，通道可以按照您喜欢的方式命名——它是由 Pusher 按需创建和处理的，不需要提前定义。你只通过服务器在频道上发送事件，每个连接到你的频道的人都会收到这些特定的事件。

步骤 3:添加授权端点

推动器中的授权非常简单:

### 您为每个客户端提供一个授权端点(例如在前端或 iOS 应用程序上)，

当客户端想要连接时，它使用客户端定义的参数访问端点，

*   您的端点应验证这些参数(例如，检查授权令牌)，并使用您选择的参数通过 Pusher singleton 对象验证连接，例如，将在连接到此通道的其他客户端之间共享的关于当前授权用户的更多数据。
*   每个客户端可以在初始化期间定义其授权端点，例如在前端应用程序或 iOS 应用程序中，您可以使用指向https://myapp.com/api/pusher/auth的授权端点来初始化推送器。
*   客户端还可以配置它们发送到该端点的参数，例如，您可以向您的 /api/pusher/auth 端点发送前端应用程序中使用的当前用户 ID 和 auth 令牌。

让我们称我们的端点为 /api/pusher/auth 。有一个操作需要实现。

安全、简单、可靠。

步骤 4:调试和验证

您可以轻松地调试和验证您的 Pusher 实现，根本不需要任何前端。在我的应用程序中，基本上没有业务逻辑，我创建了一个简单的端点，它将根据测试需求触发 Pusher 事件。

现在，我可以使用 CURL 简单地触发 Pusher 事件:

### Pusher 的酷之处在于它在网上提供了优秀的调试控制台。这意味着你不需要一个前端就绪的解决方案——在后端工作直到你准备好！

转到 Pusher 仪表板中的调试控制台，重启服务器，观察 Pusher 是否正在获取所需的数据！

摘要

```
curl http://localhost:3000/api/pusher/trigger?channel_name=test&event=myEvent
```

你已经设置好所有你需要的东西，完全接受推杆底座的可能性。您可以处理公共频道、私人频道和在线频道(私人频道和在线频道需要授权)。这些事件可以很容易地被触发，并且您有一个可重用的 Pusher::Dispatcher 类。您刚刚学习了 Pusher 调试控制台，这是您工具箱中的必备组件。干得好！

Go to your Debug Console in the Pusher dashboard, restart your server and observe if Pusher is getting the needed data!

### Summary

You have set up all you need to embrace Pusher base possibilities fully. You can handle public, private and presence channel (private and presence demands authorisation). The events can be triggered easily, and you have a reusable Pusher::Dispatcher class. You've just learned about Pusher Debug console, which is a must-have in your toolbelt. Good job!