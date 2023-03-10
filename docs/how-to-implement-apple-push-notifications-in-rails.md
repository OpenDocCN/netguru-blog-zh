# 如何在 Rails 中实现苹果推送通知

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-to-implement-apple-push-notifications-in-rails>

### 第二步

如果您还没有它——创建一个设备模型，将用户的唯一设备 id (uuid)存储在数据库中。

[代码]

 ### 第三步

为设备型号添加迁移。在我们的例子中,“uuid”列和对用户表的引用是必需的，其余的取决于您的规范。

[代码]

运行迁移。

第四步

### 您还需要准备一个操作，在这个操作中将创建用户的设备并保存到数据库中。您可以在用户注册或移动应用程序调用某个 API 端点时执行此操作。
为了创建设备对象，我们将使用 SecureRandom.uuid 方法:

第五步

```
Device.create(user: user_id, uuid: SecureRandom.uuid)
```

### 如果你使用环境变量，休斯顿将自动读取它们，所以你可以添加 APN 证书和 APN 证书密码到你的配置文件。本节查看更多配置选项:[https://github.com/nomad/houston#configuration](http://web.archive.org/web/20221202100501/https://github.com/nomad/houston#configuration)。

如果你使用 AppConfig 来存储变量，你可以在初始化 APN 连接时定义它们。下面是一个片段的例子:

[代码]

请注意环境选项:

您可以用同样的方式初始化生产:

```
Houston::Client.development
```

第六步

```
Houston::Client.production
```

 ### 是时候创建一个提醒“你好，世界！”的通知了给我们用户的消息。

创建自定义通知时，有几个选项可供选择:

```
notification = Houston::Notification.new(device: token)
notification.alert = "Hello, World!"
```

```
notification.alert = "Hello, World!
``` 

| 设置提醒信息 | 

```
notification.badge = 57
```

 | 更改徽章数量 |
| 

```
notification.sound = "sosumi.aiff"
```

 | 自定义通知声音 | 

```
notification.category = "INVITE_CATEGORY"
```

 |
| 添加类别标识符 | 

```
notification.content_available = true
```

 |  |
| 指示可用的报刊亭内容 | 

```
notification.custom_data = {foo: "bar"}
```

 | 传递自定义数据 |
| 无声通知: | 如果您想在没有声音效果的情况下提醒用户，您必须将声音设置为空字符串: | 要了解更多选项，请前往休斯顿 Github 页面。 |
|  | 第七步 | 一切看起来都很好，所以现在，使用步骤 4 中定义的 APN 方法，我们可以调用将向您的设备发送推送通知的操作: |

重要提示:如果你想测试在你的设备上接收通知-确保你的应用程序在后台或关闭。否则，该消息不会显示在您的屏幕上。

您还可以通过附带的命令行 APN 方法测试通知是否正常工作。

```
Houston::Notification.new(:sound => '', :content_available => true)
```

使用 APN 推送方法，将设备 uuid 作为参数，并定义

-c path_to_your_certificate 和-m“您的消息”

### 第 8 步(生产)

苹果鼓励通过同步连接到他们的服务器使用排队后台作业。这就是为什么休斯顿允许我们与他们的服务器建立持久的连接。

```
apn.push(notification) 
```

您可以这样创建后台作业:

[代码]

注意用于建立连接的苹果 _ 生产 _ 网关 _URI 。
同样，您也可以连接到开发网关:

摘要

谢谢你走到最后！我希望我的指南对您的开发工作有所帮助。总而言之，在本教程中，您已经学习了:

```
$ apn push "<uuid>" -c /path/to/apple_push_notification.pem -m "Hello from the command line! " 
```

### 如何准备您的应用程序来管理独特的用户设备，

如何设置休斯顿向苹果设备发送推送通知，

如何安排在后台作业中发送通知。

[code]

Notice the APPLE_PRODUCTION_GATEWAY_URI used to establish a connection.
Again - you can also connect to the development gateway:

```
Houston::APPLE_DEVELOPMENT_GATEWAY_URI
```

### Summary

Thanks for reaching to the end! I hope my guide will be helpful in your development efforts. To sum up, in this tutorial you have learned:

*   how to prepare your application for managing unique user devices,

*   how to set up Houston to send push notifications to Apple devices,

*   how to schedule sending notifications in a background job.