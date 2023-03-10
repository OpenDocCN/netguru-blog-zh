# 登录苹果——如何让你的产品留在苹果商店

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/sign-in-with-apple>

 苹果公司推出了自己的认证服务，登录苹果。这并不奇怪，因为它的主要竞争对手已经通过谷歌、脸书和 Twitter 的“登录”解决方案主导了市场。

虽然有一种替代的用户认证方法总是一件好事，但这家科技巨头将**要求大多数 iOS 应用**提供与苹果的登录。因此，如果你运行一个 iPhone 应用程序，请阅读这篇文章，了解你是否需要在 2020 年 4 月的截止日期之前将其包括在内。

## 使用 Apple 登录的优点和缺点

该服务于 6 月 3 日在 2019 年全球开发者大会的主题活动上宣布。登录苹果是一种新的安全认证方法，允许用户使用他们的苹果 ID 为应用程序和网站创建帐户，因此他们**不必向应用程序的所有者透露任何个人信息**。苹果的服务类似于谷歌或脸书的认证服务。

![why to sign in Apple](img/3777ecddfc0cad56d4e28388a9139cae.png)

*来源:[苹果面向开发者](http://web.archive.org/web/20221007200312/https://developer.apple.com/sign-in-with-apple/)*

苹果为什么要发布自己的“签到”？该公司希望它更容易使用，并通过隐藏我的电子邮件等功能为私人数据提供更好的保护。

当你使用苹果实现登录时，你的应用用户将不必浪费时间填写表格，验证电子邮件地址，或者选择和记住新密码。您将收集和存储的唯一数据是用户名和电子邮件地址。

苹果将高数据保护标准视为其相对于谷歌和脸书的竞争优势，更多的控制和安全性是使用苹果认证解决方案的主要好处。用户不仅可以在与应用程序所有者分享之前编辑他们的姓名，还可以使用**隐藏我的电子邮件功能**。它允许应用程序用户隐藏他们的电子邮件，并使用由服务生成的唯一随机电子邮件地址**登录苹果。**

启用 Apple 登录是一个更新和优化用户流量的好机会。在检查应用的一些基本功能后，您可以为用户提供登录和创建帐户的选项。一个好的做法是当用户想要保存进度或建立一个配置文件时引入它。

## 需要和苹果实现签到吗？

并非每个 iOS 应用程序所有者都需要实现与苹果的登录。如果你的应用程序使用第三方或社交登录服务，你必须这么做。这类服务有很多，包括脸书登录、谷歌登录、亚马逊登录、微信登录、Twitter 登录和 LinkedIn 登录。

如果你正在使用其中的任何一个，你需要在 2020 年 4 月前加入一个苹果登录选项。

**如果**您的应用程序:

1.  使用**您自己的帐户设置和登录系统**。
2.  要求用户使用现有的**教育、商业或企业帐户**登录。
3.  使用**政府或行业支持的**公民身份识别系统或电子 ID 来验证用户。
4.  是第三方服务的**客户端，要求用户使用其电子邮件、社交媒体或第三方账户直接访问其内容。**

## 如何启用 Apple 登录

App Store 合规规则很简单:

1.  上传到 App Store 的新应用程序必须遵循[指南](http://web.archive.org/web/20221007200312/https://developer.apple.com/app-store/review/guidelines/#sign-in-with-apple)才能被审核者接受。
2.  现有应用程序和更新必须在 2020 年 4 月前遵循该指南

启用 Apple 登录很容易。您不必处理电子邮件验证，因为所有注册该服务的用户都已经过验证。它适用于所有苹果设备，包括 iPhone、iPad、iPod touch、Mac、Apple TV 和 Apple Watch。网页和安卓应用可以使用[用苹果 JS](http://web.archive.org/web/20221007200312/https://developer.apple.com/documentation/signinwithapplejs) 登录。

苹果通过结合设备上的机器学习、账户历史和使用隐私保护机制的硬件证明来检查此人是否真实(仅适用于 iOS 设备)。

你所需要做的就是按照苹果的指示去做。首先你需要一个**苹果开发者计划**；只有到那时，你才会收到用户的数据，因为无论是免费版还是企业版都不允许登录苹果。

*   首先，阅读官方的[指南](http://web.archive.org/web/20221007200312/https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)，并确保你的开发团队已经阅读了它们。
*   其次，[注册您的域名](http://web.archive.org/web/20221007200312/https://help.apple.com/developer-account/?lang=en#/devf822fb8fc)以便在隐藏我的电子邮件选项打开的情况下向用户发送消息。
*   第三，在证书中添加一项功能。转到标识符和配置文件->标识符-> <your application="">。</your>

你也可以让你的用户通过谷歌的 [Firebase](http://web.archive.org/web/20221007200312/https://firebase.google.com/docs/auth/ios/apple) 或者亚马逊的 [AWSCognito](http://web.archive.org/web/20221007200312/https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-configuring-federation-with-social-idp.html) 进行认证。

## 我需要做很多更改才能登录 Apple 吗？

这取决于你的应用程序，但通常 iOS 开发者只需要**添加一个按钮，并在应用程序和后端处理用户认证**过程。

对于应用内部的自定义实现，请要求您的开发人员使用官方的[文档](http://web.archive.org/web/20221007200312/https://developer.apple.com/sign-in-with-apple/get-started/)。

如果你仍然对即将到来的截止日期感到紧张，请告诉我们。在 Netguru，我们有[经验丰富的 iOS 开发人员](/web/20221007200312/https://www.netguru.com/services/ios-mobile-app-development),他们会帮助你实现，并帮你解决这个问题。