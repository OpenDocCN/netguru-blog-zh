# 让你的 iOS 应用更安全的 5 个步骤

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/5-steps-to-make-your-ios-app-more-secure>

 安全是一件重要的事情。不仅是金融类 app，所有都是。你应该一直记在心里。我想给你介绍 5 个简单易行的步骤，但能让你的 iOS 应用更安全。

## 1.将机密数据存储在安全的地方

就存储机密值而言， **Keychain 是唯一正确的答案**。当你处理首选项时，用户默认值是好的，但是你不应该在其中存储凭证或个人数据。Keychain 可能看起来更难，但是使用包装器会使它更容易。我个人推荐[锁匠](http://web.archive.org/web/20221006051551/https://github.com/matthewpalmer/Locksmith)。

请记住，Keychain 是使用硬件模块(在现代设备、A7 和更新的芯片上)保护的**，并且备份到 iCloud(这是非常酷的副作用)。**

如果你想加密你的数据库，这也是可能的。对于核心数据来说有点困难，但是对于领域来说确实很简单。

你也可以使用四种不同的保护级别来编写你的文件。默认级别很好，但是您可以轻松地使您的文件更加安全。你可以在这里了解更多关于[的信息。](http://web.archive.org/web/20221006051551/https://developer.apple.com/documentation/uikit/core_app/protecting_the_user_s_privacy/encrypting_your_app_s_files)

## 2.使网络层无懈可击

**强烈不推荐 HTTP**。由于 iOS 9.0 应用传输安全(ATS)默认是启用的，所以你必须使用 HTTPS 而不是不加密的 HTTP。不幸的是，ATS 很容易被禁用。如果应用程序处于开发阶段，并且您的服务器还没有提供 SSL，这没什么，但是应用程序商店构建不应该调用 HTTP 请求，并且应该启用 ATS。

根据 NowSecure 的数据，2016 年 12 月，201 个下载量最大的免费 iOS 应用中有 80%选择退出 ATS。我真诚地希望今天会好得多。

HTTPS 很有效，但是不能保护你免受一种叫做*中间人*的攻击。**SSL pin**确实如此。这是一个非常简单的技巧，尤其是如果你使用 [Alamofire](http://web.archive.org/web/20221006051551/https://github.com/Alamofire/Alamofire/blob/master/Documentation/AdvancedUsage.md#security) 进行网络交流的话。唯一的缺点是，每当服务器的 SSL 密钥改变时，应用程序必须更新。

## 3.想想你的秘密(比如 API)密钥

它们**不应该存储在存储库**中。相反，你可以使用 [cocoapods-keys](http://web.archive.org/web/20221006051551/https://github.com/orta/cocoapods-keys) 来混淆它们。一个**混淆**对于一个专业的应用程序破解者来说可能没什么大不了的，但至少你的原始秘密值不是 Git 历史的一部分。

在 GitHub 上对 *client_secret* 的[搜索显示，有超过 40，000 个提交可能会暴露 API 密钥和秘密。不要推这些东西。](http://web.archive.org/web/20221006051551/https://github.com/search?q=client_secret&type=Commits)

现在你知道它对公共存储库极其重要，但同样适用于私有存储库。

## 4.小心第三方集成

这很困难，但是你应该尽最大努力确保第三方框架不容易受到攻击。最简单，但不是 100%有效的方法是**保持他们更新**到最新的稳定版本。您尤其应该确保您使用的 ad 库(如果有的话)是安全的。永远不要为他们禁用 ATS，即使他们礼貌地请求。

## 5.不断学习

当然，这还不是全部。您仍然应该了解最新的新漏洞。这个[库](http://web.archive.org/web/20221006051551/https://github.com/felixgr/secure-ios-app-dev)可能会有所帮助，因为它包含了 iOS 应用程序中最常见的漏洞列表。

在总结之前，我想再和大家分享两个小技巧。

我坚信你的生产应用不应该打印(或登录 Obj-C 命名)重要的语句。它们是为调试而设计的，但是黑客很容易看到它们。

你也不应该忘记 Xcode 的静态分析报告。你可以通过按⇧⌘B.得到它。这个工具有助于揭示逻辑缺陷，内存泄漏，未使用的变量和其他错误。

## 总结

即使遵循这些步骤也不意味着你的 iOS 应用程序 100%不会受到攻击。开发者和破解者之间有一场永恒的战争。我希望这篇博文能成为有用的武器。