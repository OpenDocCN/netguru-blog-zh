# 如何设置 Callio，Slack-Twillo 集成

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/callio-setup-slack-twillo-integration>

 最近，我们发现我们可以[改善内部流程，接听主要来自国外的客户电话](http://web.archive.org/web/20221007094317/https://www.netguru.com/blog/introducing-callio-track-twilio-calls-slack)。我们的想法是建立一个简单的[应用](http://web.archive.org/web/20221007094317/https://www.netguru.com/callio)，它将拦截来电，发布一个空闲通知，并允许我们通过使用 Twilio number 的网络浏览器来接听这些电话。Callio 就是这样诞生的。在这篇文章中，你会发现一个关于它如何工作以及如何将它与你的账户相结合的综合指南。

### 好处

Callio 是一款极其轻量级的应用。它使用 Sinatra 与 [Slack](http://web.archive.org/web/20221007094317/https://slack.com/) 和[Twilio](http://web.archive.org/web/20221007094317/https://twilio.com/)API 进行通信，使用一个简单的 web 客户端和一些 JavaScript 来处理浏览器中的语音呼叫，并使用几个 webhooks 来使其顺利运行。

*   办公室里不需要真正的电话——你只需要一个网络浏览器，不需要额外的设备。

*   对于找不到你的号码的客户来说没有问题——由于 Twilio，你可以从几十个国家中选择一个号码。

*   空闲频道上的任何人都可以接听电话。

*   Callio 保留了你所有电话的完整历史记录以及每一个电话的录音。如果你错过了什么，你仍然可以在 Slack 上看到有人试图联系你的消息

### Callio 在实践中如何工作

1.  有人打电话给你。

2.  Callio 会在你的 Slack 频道上发布一条通知，并提供一个网络浏览器链接，你可以在那里接听电话。

3.  您点击通知中的链接。

4.  当你点击浏览器中的“呼叫”按钮时，Slack 会发布另一条通知，让团队的其他成员知道有人已经接听了电话。

5.  呼叫被执行并被记录。

6.  当其中一方挂断时，Slack 会发出通知，然后线路就可以再次使用了。

7.  通话详情和录音被添加到通话记录中

### 如何设置 Callio

#### **Twilio 账户**

要开始使用 Callio，你必须注册使用[【Twilio】](http://web.archive.org/web/20221007094317/https://www.twilio.com/)，并选择一个你想使用的号码。它是免费的，为测试目的提供了完整的功能-你只会收到一条短信，告知你正在运行一个试用帐户。

#### **Twilio 传入连接 webhook**

在 Twilio 上注册并选择号码后，你必须配置你的第一个 webhook。为此，将你的 Callio 应用的地址粘贴到电话号码下，然后:配置- >语音- >有电话进来

#### **用于完成连接的 Twilio web hook**

在粘贴 Callio 来电 webhook 的同一个地方，您可以输入端点地址，通知用户通话已经结束。它位于“通话状态更改”输入框下(见上面的截图)。

#### **Twilio 应用程序**

完成号码后，进入可编程语音- >工具- > TwiML 应用程序，创建一个应用程序。您的 Twilio 应用程序将在几秒钟内准备就绪(稍后您将需要它的 SID)。接下来，您可以配置一个 webhook 来处理语音呼叫，只需在 voice 请求的 URL 中键入您的 Callio 地址。

#### **松弛应用**

当我们完成 Twilio 后，是时候创建一个简单的 Slack 应用程序了。去 https://api.slack.com/apps[](http://web.archive.org/web/20221007094317/https://api.slack.com/apps)创建一个 app。你还需要它的令牌。请记住为此应用程序设置适当的权限，以便能够在您的频道上发布消息。我们需要的唯一权限是 chat:write:bot。

#### **Twilio-Slack-Notifier 设置**

你必须在 config.yml 文件中提供一些秘密数据。一个示例配置文件应该类似于下面的内容:

slack:  client _ token:' xoxp-1234-your 0 slack-99 secret 4 token-nom nom 3 '  频道:' awesome-slack-channel-name '  ' apand 8 your 9 app 3 sid 9 from 1 twilio '  client _ name:' your-client-name '  caller:'+

那是什么？我从哪里得到所有这些数据？

*   Slack 客户端令牌:这是一个在您使用应用程序时授权您的 Slack 帐户的令牌。首先，你要创建一个 Slack 应用程序(只需如上给它一个名字和权限)，当你在[【https://api.slack.com/apps.】](http://web.archive.org/web/20221007094317/https://api.slack.com/apps)下选择你的应用程序时，你就会得到令牌

*   Slack channel:它只是您希望发布通知的频道名称。只要通道确实存在，它可以是任何东西。

*   Twilio 账号 SID:登录后在你的 Twilio 仪表盘中获取:[【https://www.twilio.com/console】](http://web.archive.org/web/20221007094317/https://www.twilio.com/console)

*   Twilio auth token:如上:[【https://www.twilio.com/console】](http://web.archive.org/web/20221007094317/https://www.twilio.com/console)

*   Twilio 应用 SID:你必须先创建一个 Twilio 应用。进入控制台- >可编程语音- >工具- > TwiML 应用。创建新的应用程序后，您将在那里获得应用程序 SID。

*   Twilio 客户端名称:这是您必须在 Twilio 仪表板和 Callio 配置中键入的名称。它授权浏览器连接到您的 Twilio 并执行电话呼叫。

*   来电者:你的 Twilio 电话号码。

*   web 客户端链接:您的 Web 客户端的端点地址。这是发布在 Slack 频道上的链接。

你已经准备好了！现在你不会错过任何来电。