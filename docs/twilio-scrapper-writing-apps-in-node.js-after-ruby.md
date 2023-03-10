# 继 Ruby 之后，在 Node.js 中引入 Twilio Scrapper -编写应用程序

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/twilio-scrapper-writing-apps-in-node.js-after-ruby>

 在这篇博文中，我想和你分享我在没有服务器端 JavaScript 经验的情况下，用 Node.js 编写一个实验性应用的印象。我是一名 Ruby on Rails 开发人员，在 Ember.js 方面有着丰富的经验——所以是时候试试 server JS 了！

背景

## 几周前，我遇到了以下情况——我需要在 GitHub 上使用一个公司共享的(但很少使用的)帐户，该帐户启用了双重认证。原来，物理电话在我们的办公室，每次我需要以匿名模式登录帐户时，我都必须要求我们的办公室团队在 Slack 上向我发送代码。我不喜欢这样，在和几个队友集思广益后，我决定做一个应用程序来解决这个问题！。**让我们创建一个共享的 Twilio 电话号码，该号码将设置在我们的共享 2FA 帐户**上，并将所有消息存储在应用程序中，然后向通过我们的 netguru.pl 域登录 Google 帐户的用户公开这些消息，同时向 Slack 发布通知，告知我们的 Twilio 号码上有新消息。

我不相信这个解决方案对我们有好处，因为它降低了安全级别。为了增加一个好处，以防该应用程序最终没有在全公司范围内使用，我决定将它写在 node.js 中，这样至少我可以在这个过程中学习一些新的东西。

与 express.js 的首次会面

## 我对 express.js 的第一印象是惊喜。有教程可用，但大多数都强调这只是你用来实现目标的一个选项。与 Rails 相比，它在开始时有点令人困惑，因为 Rails 从一开始就非常简单。然而，我最终得出结论，node.js 中的**编码类似于 Sinatra**——只采用低级别的请求和响应对象，并按照自己的方式做所有事情。实际上，很鼓舞人心。

发展和资源

## 我从我在 Pluralsight 上找到的两个教程开始- [这里](http://web.archive.org/web/20221007200244/https://www.pluralsight.com/courses/nodejs-express-web-applications)和[这里](http://web.archive.org/web/20221007200244/https://www.pluralsight.com/courses/oauth-passport-securing-application)。我花了几个小时浏览它们，但是我重复使用了我在应用程序中学到的大部分概念。我不喜欢缺少 ES6，但你第一次体验一项技术并不是尝试的时候。

当使用 Ember.js 时，我发现 JavaScript 非常令人兴奋，我必须说，用纯 JavaScript 开发应用程序非常相似。前端应用程序中存在的函数式编程和原型继承基本上在代码中无处不在，这增强了我的信心，即关于 JavaScript 基础知识的**对于每个开发人员都是必须的——前端和后端**。最后，但并非最不重要的是-这个应用程序基本上是一对夫妇的文件和非常少的代码行，它有一个非常简单的设置和工作！

我真正喜欢这次经历的是函数式编程。我没有使用任何助手，全局变量或奇怪的方法。**一切都只是作为回调函数**中的一个参数，所以我发现学习过程很好也很简单。我阅读了 express.js 的 API，并且确切地知道我正在使用的函数中哪些是可用的，哪些是不可用的。

结论

## 作为一个总是从支持的角度考虑 Rails 的人，尝试 node.js 几天是很有趣的。我肯定会努力扩展我在这方面的经验，并最终编写更大的代码。如果你想看 Twilio Scrapper 的代码，[可以在 Github](http://web.archive.org/web/20221007200244/https://github.com/netguru/twilio-2fa-bot) 上查看。此外，我还证实了如今经常被重复的一句话——JavaScript 既是后端又是前端，而且将会存在很长一段时间。

你每天都在使用 node.js 吗？你愿意分享你的观点吗？

Are you working with node.js everyday? Would you like to share your opinions?