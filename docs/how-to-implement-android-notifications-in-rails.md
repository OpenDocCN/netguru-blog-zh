# 如何在 Rails 中实现 Android 通知

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-to-implement-android-notifications-in-rails>

 移动通知是你的应用程序的一大特色——尤其是当你集成了移动和桌面解决方案的时候。下面是如何在 Ruby on Rails 项目中设置 Android 通知的快速指南。

我假设你的应用程序中有某种类型的用户标识。你正在使用设计宝石。

您将使用的内容:

## [https://github.com/spacialdb/gcm](http://web.archive.org/web/20220927120300/https://github.com/spacialdb/gcm)-一个围绕谷歌云消息 API (GCM)的简单包装器，

*   android 项目 id 和 API key——可以在这里获得[，](http://web.archive.org/web/20220927120300/https://developers.google.com/cloud-messaging/#create-proj)
*   一些 android 应用程序注册到 GCM，并将注册 id 发送到后端 API。
*   优点:

### 实现起来非常简单——不需要魔法，

*   小型库——179 行代码，
*   利用谷歌云信息，
*   安卓和 iOS 都可以用(我们只测试了安卓)。
*   缺点:

### 如果您需要非常低的延迟，Google Cloud Messaging 可能不适合您——参见 [Messenger 案例研究](http://web.archive.org/web/20220927120300/https://www.facebook.com/notes/facebook-engineering/building-facebook-messenger/10150259350998920)。

*   第一步

添加到 Gemfile:

### 并运行 bundle install。

第二步

```
gem ‘gcm’
```

将 API 密钥添加到 sec-config.yml :

第三步

### 现在，我们可以利用 GCM 发送一些通知。在下面的代码片段中，您可以找到一些示例服务对象，它们接受注册 id(需要由移动客户端推送到您的后端 API)和一个消息散列，该消息散列只是一个将包含在通知中的消息。

[片段]

```
environment:
  google_gcm_api: some1cool1key1here
```

第四步

### Android 客户端需要使用广播接收器来监听这些通知。它将如何处理这些通知取决于您的应用程序的业务逻辑。这种应用的示例代码可以在[这里](http://web.archive.org/web/20220927120300/https://github.com/googlesamples/google-services/tree/master/android/gcm)找到。

希望你喜欢！如果您有任何意见或想法如何改善它，请在下面分享。

Now, we can make use of GCM and send some notifications. In the snippet below, you can find some example service object that takes registration IDs (that need to be pushed by mobile clients to your backend API) and a message hash which is just a message that will be included in the notification.

[snippet]

### Step 4

Android clients need to listen to those notifications by using the broadcast receiver. What it will do with those notifications will depend on the business logic of your application. A sample code for such an application can be found [here](http://web.archive.org/web/20220927120300/https://github.com/googlesamples/google-services/tree/master/android/gcm).

I hope you like it! If you have any comments or ideas how to improve it, please share them below.