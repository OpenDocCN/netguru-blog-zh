# iOS 中的 HTTP 调试通过 ResponseDetective 变得更加容易

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/response-detective-ios-open-source>

 我们很高兴宣布我们新的开源 iOS 工具 HTTP de

窃听-响应检测。

你在请求完成回调中放了多少次`NSLog`？你有多少次使用断点来检查发送给你的 API 服务的头和体？现在，那些都是过去的事了！我们很高兴宣布我们新的开源 for HTTP 调试工具—[response detective](http://web.archive.org/web/20221007091626/https://github.com/netguru/ResponseDetective)。

网络层的夏洛克·福尔摩斯

## 有时会出现问题——这很正常，[当同时开发 iOS 移动应用](/web/20221007091626/https://www.netguru.com/services/ios-mobile-app-development)和后端 API 时，这种情况经常发生。在这种情况下，快速检查网络层发生了什么非常重要——发送和接收了哪些报头，以及进出了哪些有效载荷。以前，您必须在 API 客户端实现内部手工注入断点和日志。这是可行的，但是它既不可维护，也不可扩展。输入 ResponseDetective。您所要做的就是用几行代码设置所需的拦截器，并将一个`NSURLProtocol`注入到您的会话的`NSURLSessionConfiguration`中。

好的基础

## ResponseDetective 没有使用私有的 API，只是在`NSURLProtocol`之上的一个花哨的层，这个类从 iOS 2.0 就存在了，被[的许多](http://web.archive.org/web/20221007091626/https://github.com/kylef/Mockingjay) [请求存根](http://web.archive.org/web/20221007091626/https://github.com/AliSoftware/OHHTTPStubs) [库](http://web.archive.org/web/20221007091626/https://github.com/luisobo/Nocilla)使用。结果是，如果你使用的是`NSURLSession`，每次你执行一个 HTTP 请求时，它会要求每个注册的 NSURLProtocol 执行它，如果它们可以的话。

ResponseDetective uses no private APIs and is just a fancy layer on top of `NSURLProtocol`, a class which has existed since iOS 2.0 and is used by [many](http://web.archive.org/web/20221007091626/https://github.com/kylef/Mockingjay) [request stubbing](http://web.archive.org/web/20221007091626/https://github.com/AliSoftware/OHHTTPStubs) [libraries](http://web.archive.org/web/20221007091626/https://github.com/luisobo/Nocilla). Turns out, if you’re using `NSURLSession`, every time you execute an HTTP request, it asks each registered NSURLProtocol to perform it if they’re able to.

![](img/aaddf13403e897c311e771c960aa5b9c.png)

这就是反应检测的用武之地。它提供了`NSURLProtocol`的子类，该子类首先拦截请求，执行请求，最后拦截响应。说到截击机...

定制的新水平

## 默认情况下，ResponseDetective 不做任何事情。是你，用户，决定它如何工作，拦截什么。想要打印所有发出的 JSON 请求吗？没问题！

想要将所有 XML 响应转储保存在一个数组中吗？给你！

也许你想利用你那令人敬畏的 [CocoaLumberjack](http://web.archive.org/web/20221007091626/https://github.com/CocoaLumberjack/CocoaLumberjack) 装置？再简单不过了！只需实现自己的输出流类型，并将其作为`outputStream`参数传递。

你甚至可以创建自己的拦截器，倾倒任何你想要的东西。只要执行一个相关的协议，你就可以开始了。

技术挑战

## 对于这么小的项目，所有的 API 都应该尽可能简单，我们不希望有任何障碍。正如你可能猜到的，事实并非如此。

首先引起我们注意的是`NSURLProtocol`的接口，原来是，不是基于实例，而是基于类。对我来说，我认为[共享易变状态是敌人](http://web.archive.org/web/20221007091626/https://twitter.com/teozaurus/status/518071391959388160)，这是一个巨大的失望。我们不得不根据类方法来重新思考我们的 API。

一开始，我们决定我们的库将没有依赖项，并且将用纯 Swift 编写。一切都很顺利，直到我们不得不在 libxml 之上实现一个 [XML 修饰器](http://web.archive.org/web/20221007091626/http://stackoverflow.com/questions/19857045/pretty-print-xml-from-nsstring-in-objective-c)。

In the beginning, we decided that our library would have no dependencies and would be written in pure Swift. It was all going well until we had to implement an [XML prettifier](http://web.archive.org/web/20221007091626/http://stackoverflow.com/questions/19857045/pretty-print-xml-from-nsstring-in-objective-c) on top of libxml.

![](img/4bfde65ed8afa9a8d071bd0cbfcc88b2.png)

好了，让我们创建一个桥接标题，然后`#import <libxml/tree.h>`就这样！

Alright, let’s create a bridging header and `#import <libxml/tree.h>` there!

![](img/705a265100bf5bc0b8b5b5204ca89e21.png)

就在这时，我们想起 Swift 模块不支持任何静态库连接。我们别无选择，只能在 Objective-C 中实现修饰器(违反了我们声明的目标),并使用桥接头将函数导出到 Swift，顺便说一下，[在框架目标](http://web.archive.org/web/20221007091626/http://stackoverflow.com/questions/24875745/xcode-6-beta-4-using-bridging-headers-with-framework-targets-is-unsupported)中应该是不可能的。

思考不同

## ResponseDetective 不同于其他开源项目的另一点是它经过重新思考的设置。

我们没有包含一个演示应用程序，它没有特定的用途，并且每次用户想玩这个框架时都需要重新编译，我们决定包含一个演示 [playground](http://web.archive.org/web/20221007091626/https://developer.apple.com/library/ios/recipes/Playground_Help/Chapters/AboutPlaygrounds.html) 。这开辟了一条包含丰富文档和真实代码示例的道路，这是向潜在用户介绍框架的最佳方式。

此外，ResponseDetective 利用了预先构建的 [Carthage](http://web.archive.org/web/20221007091626/https://github.com/Carthage/Carthage) 依赖项。结果？由于 Carthage 不构建依赖项(不像 CocoaPods)，我们设法实现了空前的记录——在我们的持续集成服务器上构建 42 秒。

这只是开始

ResponseDetective 是一个新项目，仍然有很多工作要做，因为我们不断提出新的想法和集思广益的改进。

![](img/b3cc98b2065931dc2a42008c91717de5.png)

最后但同样重要的是，没有开放就没有开源——如果您有任何评论、问题或改进的想法，请随时提出问题并为项目贡献力量。

*我们正在进行更多的[开源项目](http://web.archive.org/web/20221007091626/https://www.netguru.com/resources)——请随意使用我们已有的资源，或者加入我们。我们也很想在推特上见到你-[@网络大师](http://web.archive.org/web/20221007091626/https://twitter.com/netguru)。*

## It’s Just The Beginning

ResponseDetective is a fresh project and there’s still a lot of work to do as we’re constantly coming up with new ideas and brainstorming improvements.

Last but not least, there is no open source without openness – if you have any comments, questions or ideas for improvements, feel free to open an issue and [contribute to the project](http://web.archive.org/web/20221007091626/https://github.com/netguru/ResponseDetective).

*There are more [open source projects](http://web.archive.org/web/20221007091626/https://www.netguru.com/resources) we’re working on - help yourself to what we’ve got, or join us. We'd love to meet you on Twitter, too - [@netguru](http://web.archive.org/web/20221007091626/https://twitter.com/netguru).*