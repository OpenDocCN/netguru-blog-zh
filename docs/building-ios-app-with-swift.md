# 使用 Swift 构建 iOS 应用程序的利弊

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/building-ios-app-with-swift>

 在编程界，Swift 可能被认为只是一个孩子，但它正成为希望开发 iOS 应用的企业的流行语言。

Swift 易于使用和阅读，改变了 iOS 应用程序的开发方式，正在创造不可能的事情。

在 Swift 出现之前，应用程序主要是用 Objective-C 创建的，既耗时又困难。有了 [Swift](http://web.archive.org/web/20221007100252/https://www.netguru.com/blog/swift-vs-objective-c) ，它为 app 开发带来的好处是数不胜数的。速度、安全性、用户体验和维护只是几个例子。当我们把注意力转向在 [iOS 应用程序开发中使用 Swift 编程语言时，让我们考虑这些。](http://web.archive.org/web/20221007100252/https://www.netguru.com/services/ios-mobile-app-development)

## 什么是 Swift？简要概述

Swift 由苹果公司于 2014 年创建，是一种适用于 iOS、iPadOS、macOS、tvOS、watchOS 和 Linux 应用程序的编程语言。然而，直到 2015 年，苹果才决定开源代码，以促进更大的采用。现在，苹果应用商店有[196 万个应用可供下载](http://web.archive.org/web/20221007100252/https://buildfire.com/app-statistics/#:~:text=The%20Apple%20App%20Store%20has,on%20the%20Google%20Play%20Store.)，其中部分是用 Swift 编写的。其中包括 Airbnb、脸书、LinkedIn、Lyft、Slack 和优步。

Lyft 之前是用 Objective-C 语言编写的，这是一种可以追溯到 20 世纪 80 年代的编程语言，它是应用程序转向 Swift 的一个例子。Lyft 的最初版本已经增长到 75，000 行代码，但通过迁移到 Swift，该公司用不到三分之一的代码重新创建了具有相同功能的应用。

几年之内，社区已经从过时的 Objective-C 语言转变为一种现代的、向后兼容的编程语言，这种语言易于学习和阅读。对于寻求一种编程语言来开发应用程序的企业来说，以下是 Swift 的利与弊。

## Swift 的优势

Swift 通常被称为“Objective-C，没有 C”，与其前身相比有许多优势。

### 它很快

苹果官方网站声称 [Swift 比 Objective-C](http://web.archive.org/web/20221007100252/https://www.apple.com/swift/) 快 2.6 倍，有助于节约成本措施。读写也比较容易。如上文 Lyft 示例所述，企业可以使用 Swift 快速开发 iOS 应用程序，因为它需要的代码比其他语言少得多。

很安全

Swift 的另一个优势是它的安全性。它是一种类型安全和内存安全的编程语言。类型安全指向防止任何类型错误的语言。内存安全意味着它避免了与未初始化指针相关的漏洞，这可能导致程序崩溃。有了更短的反馈循环(这是当输出作为输入反馈时，这决定了该循环的原因和效果)，开发人员可以看到任何代码错误，减少了花费在调试上的时间，并消除了低质量代码的风险。

![Closeup of hands of young man in checkered shirt using mobile phone while his partners arguing](img/0264b21aeeb2d8508ee10c3df58b6350.png)

可与 Objective-C 互操作

### 通过与 Objective-C 兼容，项目可以用任何一种语言编写。这对于正在更新的大项目尤其有用，因为 Swift 增加了更多的功能，然后这些功能被放入 Objective-C 的代码库。

维修费用低

### 一旦用 Swift 构建了应用程序，维护起来就很容易了。与在两个独立文件中管理的 Objective-C 相比，Swift 结合了 Objective-C 头文件(。m)和实现文件(。h)合并成一个程序(。swift)文件。需要注意的一点是，Swift 有依赖关系。然而，在 MacOS 上，Swift 已经安装完毕，可以使用了，在 Linux 上，要安装 Swift，你需要先安装某些依赖项，比如 Python。

更好的用户体验

### 使用 Swift 开发的应用程序需要更少的安装时间和设备内存，为用户提供更好的应用程序体验。

有效的内存管理

### Swift 使用一种称为自动引用计数(ARC)的解决方案，该解决方案建立在其 ObjectiveC 的前身之上。ARC 确定哪些类实例没有被使用，并为开发人员清除它们。这让开发人员有更多时间关注应用程序的性能，而不会减少其 CPU 或内存分配。

ABI 稳定性

### 应用程序二进制接口(ABI)是 Swift 应用程序编程接口(API)的二进制等价物。据 Swift 称，虽然 ABI 稳定性是任何编程语言的一项重大成就，但“Swift 生态系统的最终好处是实现应用和库的二进制兼容性。”实际上，ABI 允许使用不同版本的 Swift 甚至 ObjectiveC 编译的代码进行通信。

选项的使用

### 选项是一个编程概念，使开发人员能够防止应用程序崩溃，同时确保整个应用程序保持干净的代码。可以把它想象成一个包装器类型，它把值包装在里面。可选的既可以包含内容，也可以为空。为了确定这一点，需要解开可选选项，如果处理得当，就不会导致崩溃。

Swift 的缺点

### 虽然 Swift 旨在使开发人员更容易维护和编写应用程序代码，但也有一些缺点需要考虑。

Swift 仍然是一门新的语言

## 与自 20 世纪 80 年代以来就存在的 Objective-C 相比，Swift 是一个新来者，创建于 2014 年。这意味着它可能会遭受成长的痛苦。尽管最近对 ABI 稳定性和向后兼容性进行了更新，但 Swift 在工具和库方面仍然有限。

二进制兼容性并不总是有效的

尽管 ABI 稳定性是 Swift 5.1 的一个优势，但用不同版本的 Swift 编译的代码可能会出现问题。当开发人员主要使用 Objective-C 时，代码可能已经被编译成静态库，并作为依赖项引入到项目中。在推出 Swift 的 ABI 之前，不可能在 Swift 中创建静态库。虽然现在这是可能的，但是在项目中实现这些依赖关系还存在一些问题。

![Netguru-Biuro-2018-6078 _HD](img/bde610bcec5039d7938a663e91875906.png)

这不是一种反思性的语言

### 与 Java 或 Kotlin 等编程语言相比，Swift 不是一种反射性语言。相反，它提供了另一种选择:镜像功能。凭借这一点，Swift 可以获取一个对象并“自我描述”它，但它不能从内部操纵它。如果 Swift 中有反射，它会自动注入依赖项，但据信这很难实现。

小型人才库

### 这是另一个小缺点，但无论如何都需要提出来。虽然 Swift 的社区发展很快，但与其他编程语言相比，它仍然相对较小。StackOverflow 的 2019 年开发者调查发现，在超过 87，000 名受访者中，只有 6.6%的人使用 Swift。

学习一门新语言

### 虽然它是一种容易学习的语言，但它仍然需要时间来理解 Swift，而一些项目可能不具备这一点。如果一家企业可以推迟发布他们的应用程序，直到团队成员对 Swift 感到满意，从长远来看，这将是一个很好的举措，因为苹果将继续投资于其开发。

Swift 是 iOS 开发的未来吗？

### 尽管还很年轻，Swift 正在成为 iOS 开发中广泛使用的编程语言。已经选择 Swift 的公司有 Eventbrite、Kickstarter、LinkedIn、Twitter 和 Whatsapp。尽管仍有一些初期问题需要解决，Swift 正在变成一种成熟的语言，最终可能会取代 Objective-C 成为开发 iOS 应用程序的首选语言。

从长远来看，Swift 将帮助企业填补移动应用的空白。这是一种很容易理解的快速语言，它正在成为开发人员的首选。由于它是一个开源平台，它背后有一个庞大的社区，任何希望参与的人都可以做出贡献。对于希望以最少的努力转换到一种语言的企业来说，Swift 将继续是未来应用程序开发的关键组成部分。如果您正在寻找学习 Swift 的机会，[请给我们发送消息](http://web.archive.org/web/20221007100252/https://www.netguru.com/contact)。

## Is Swift the future for iOS development?

Despite its young age, Swift is becoming a widely used programming language for iOS development. A few of the companies that have already chosen Swift are Eventbrite, Kickstarter, LinkedIn, Twitter, and Whatsapp. Even though there are still teething issues to solve, Swift is turning into a mature language that could, eventually, see it displacing Objective-C as the go-to language for developing iOS apps.

In the long term, Swift will help businesses fill the mobile app gap. It’s a fast language that is easy to understand, it’s becoming a preferred choice for developers. As it’s an open-source platform it has a substantial community behind it with contributions from anyone who wishes to take part. For businesses looking to make the switch to a language with the least amount of effort, Swift will continue to be a key component for app development in the future. If you’re looking for an opportunity to learn Swift, [send us a message](http://web.archive.org/web/20221007100252/https://www.netguru.com/contact).