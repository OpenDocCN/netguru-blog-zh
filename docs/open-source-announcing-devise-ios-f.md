# 开源:宣布简化授权的设备

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/open-source-announcing-devise-ios-f>

 介绍 device-iOS，这是一个简单的客户端，可以很容易地与 device 集成并自动连接。![](img/b333c2fbbb77fe55cbae1a4bc2500dcf.png)

你[是在为 iOS](/web/20221208044130/https://www.netguru.com/services/ios-mobile-app-development) 开发吗？厌倦了一遍又一遍地实现授权？[设计](http://web.archive.org/web/20221208044130/https://github.com/plataformatec/devise)的灵活授权方案适合你吗？别担心，我们有你需要的东西。

介绍 device-iOS，这是一个简单的客户端，可以很容易地与 device 集成并自动连接。我们专门创建它来与 device-iOS-Rails gem 一起工作，以处理身份验证所需的一切。

## device-iOS 是做什么的？

有时候，手机上的 auth 不能独立。作为开发人员，您可能希望使用公开的 API 来验证它的服务器端。移动应用程序将需要实现一个网络层，它将与服务器通信。然后还有 UI 要考虑。一切都应该是一致的，尽可能可配置的，并且可以在几个步骤中实现。

如果这听起来像是一项耗时的任务，那是因为它确实如此。在遇到同样的问题后，我们创建了 device-iOS 来一次性解决所有问题。

它有什么特点？它拥有简单身份验证管理所需的一切。截至今天，device-iOS 处理:

*   用户注册，

*   登录/注销，

*   提醒密码，

*   表单验证，

*   档案更新，

*   帐户删除，

*   会话持续。

所有这些都由一个简单的 oAuth 令牌实现提供支持。device-iOS 也关心网络层的问题，这意味着它能够在出现连接问题时自动重试请求。当然，这种行为是可配置的，取决于开发人员的需求。

此外，device-iOS 不是由一个严格的模型定义的。如果当前状态不适合开发人员的需求，可以通过子类化轻松定制。灵活性规则！

## 如何使用 device-iOS？

如果你想看看它在实践中是如何工作的，这里是我们的回复:

*   设计-iOS -一个 iOS 可可吊舱，准备与设计宝石一起工作，

*   device-iOS-Rails 后端 gem -一个与 device 和 device-iOS pod 合作的 Rails gem，

*   这是一个带有工作 API 的示例应用程序，您可以将它与您的 iOS 实现挂钩。

为了简单和易于集成，已经将 device-iOS 提取为 gem(支持 Rails 的后端)和 [pod](http://web.archive.org/web/20221208044130/http://cocoapods.org/) (iOS)。它还提供了一个默认配置，允许开发人员立即实现一个基本的解决方案。

惟一的要求是在 iOS 端有一个服务器 URL，在后端有一个带有 device+device-iOS-gem 的 rails 应用程序。

考虑一个没有 UI 的移动应用。不可思议，对吧？别担心。为了让你的工作更快，device-iOS 提供了方便的界面组件。

## 即将推出的新功能

仍然有很多工作要做，我们不断提出新的想法和集思广益的改进。

虽然这是超级秘密，但我们可以稍微揭开秘密的面纱，宣布 device-iOS 将成为非凡事物的一部分，这将使 iOS + Rails 成为有史以来最好的一对。

加入进来！最后，请随时为项目做贡献！开源岩石！

*我们还在做更多的[开源项目](http://web.archive.org/web/20221208044130/https://www.netguru.com/resources)——请随意使用我们已有的资源，或者加入我们。我们也很想在推特上见到你-[@网络大师](http://web.archive.org/web/20221208044130/https://twitter.com/netguru)。*