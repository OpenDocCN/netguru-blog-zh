# 移动应用架构——为什么如此重要？

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/mobile-application-architecture-why-is-it-so-important>

 好的移动 app 架构是所有设计良好的软件的基础，能够提供优秀的用户体验。然而，当谈到移动应用程序或平台时，好的架构总是有问题。在 Android 的情况下，可能是因为谷歌不支持甚至不推荐任何特定的架构。另一方面，苹果建议 UIKit 采用 MVC 架构，但他们的提议引起了很多争议，许多专家声称这不是一个好的解决方案。

在这篇博文中，我们将试图找出什么是好的移动应用开发架构。

## 什么是好的移动应用架构

好的移动应用架构是一个能够加强假设和好的**编程模式的架构，比如 SOLID、KISS，或者确保组件有多个责任层。**满足这些条件可以让你**加速开发，让未来的维护变得更加容易，**从而节省你的时间和金钱。

明智选择的架构和特定于平台的技术(如 iOS 的 Swift 或 Android 的 Kotlin)**将是以最有效的方式解决复杂业务问题的最佳选择**移动项目。这将使你避免混合技术的怪癖所导致的许多问题。从长远来看，这肯定是一种省时省钱的方法。

此外，一个好的架构应该是如此抽象，以至于它可以应用在各种平台上——比如 iOS 或 Android T2。由于这样的假设，共同的目标可以以统一的方式实现，这将产生更多的时间效率。**一个好的架构最重要的特征之一是责任层分离。**这样的划分将是减少错误背景和程度的关键。

最流行的多层架构之一是**三层架构**，如下图所示:

![mobile app architecture best practices: a three-layer architecture](img/b9e1e011d9b5b19549987bef1ba6788e.png)

移动架构使用示例

## 清晰定义的架构使工作变得更容易、更快捷。开发人员和管理人员**对应用程序中的工作和数据流**有更好的控制。它使得测试更加有效，并且提高了管理和产品的质量。

查看一个三层模型(如上图),您可以看到**每一层的实施都取决于其目的或项目范围。**

表示层

### 表示层取决于屏幕设计及其行为。另一方面，业务逻辑取决于数据层将提供什么样的数据，以及需要如何处理这些数据来满足表示层的要求。最后，数据层将负责管理数据源，同步它们，并将其提供给更高的级别。

数据层

### 它有最明确的范围，这使它成为优化的完美起点。**为了开始数据层的优化，我们需要选择一个编程模式**，它将解决常见的问题，使我们的工作更加容易和快速。数据层移动项目的最佳模式是所谓的**存储库模式**，如下所示:  

It has the most specified scope, making it the perfect starting point for optimizations. **To start optimization of the data layer we need to select a programming pattern** that will solve common problems and make our work much easier and faster. The most optimal pattern for mobile projects for the data layer will be the so-called **Repository pattern,** which is shown below:  

![mobile application architecture diagram: Repository pattern](img/3f33e7448e957d3dd8c3ceece969e6ce.png)

**T** 何库模式

### 这是数据层模式的众多例子之一。作为 Netguru 的移动团队，我们认为它是大型移动项目的完美解决方案，因为它解决了管理多个数据源和将数据实体映射到业务逻辑组件使用的数据模型的问题。  应用这种方法(一种众所周知的模式)解决了另一组现成的问题。在这种情况下，使用存储库模式将允许您**创建负责为业务层提供良好映射数据的逻辑**，并且数据源的所有特定实现都可以被轻松替换，而不会对业务逻辑层产生任何影响。同样，我们**通过精心选择的模式节省了时间和金钱。**

上面给出的示例数据层是众多示例中的一个。类似的解决方案也可以在表示层找到，比如 **MVP，MVVM，MVI** 等。通过解决不同领域的另一组问题，这将使工作变得更容易。

为什么我们低估了移动应用架构

## 近年来，移动应用架构的方法有所改进。开发人员更关注已被证明的模式和标准，但是仍然有许多工程师没有遵循最佳实践。许多移动应用程序是用低质量的源代码创建的，没有任何架构，甚至是基于反模式的。这样的定制应用程序很难维护和进一步开发。糟糕的架构会 [大幅增加开发的时间和成本](/web/20220927122402/https://www.netguru.com/blog/mobile-app-development-cost-how-to-estimate-your-mobile-project) 。

许多消息来源还建议寻找一种替代本地[企业移动开发](/web/20220927122402/https://www.netguru.com/services/enterprise-mobile-app-development)的方法，这通常被认为是耗时且有问题的。在原生开发中，您至少需要两个开发人员——每个人负责一个平台，那么为什么不选择跨平台开发，比如使用 React Native？[混合技术的确让我们降低了成本](/web/20220927122402/https://www.netguru.com/blog/cross-platform-mobile-apps-development)，但它们并不适合所有的应用。忽略这一点可能会导致移动应用架构出现很多问题。

移动应用程序架构的问题

## 选择正确的架构应该是的必经步骤，也是[软件开发](/web/20220927122402/https://www.netguru.com/services/software-development)的设计和规划阶段的主要元素。然而，由于开发人员的疏忽、仓促、缺乏经验和知识，或者忽视供应商(例如 Google)，架构经常被忽视。应用缺乏架构 (不仅仅是移动) 导致软件出现重大问题:

更容易出错；

*   很难开发和维护；
*   代码可读性较差；
*   让许多开发人员同时工作可能很困难；
*   [没有架构或者设计模式的源代码更难测试](/web/20220927122402/https://www.netguru.com/blog/how-to-test-android-app)，导致关键功能的单元测试缺失。缺乏测试会导致维护软件的困难(例如，没有回归控制，更难的重构或错误修复等)；
*   还有很多。
*   **开发一个没有架构**或设计模式的软件项目可以比作**没有地基的建筑**——建筑越大，由此引发的问题就越多。

刚开始时，没有经验的开发人员或管理人员通常认为没有架构的方法更快。然而，很快这就变成了一条死胡同。通常，向这样的代码添加架构或模式为时已晚。随着项目规模和**复杂性的增加，这些问题也在积累。**

精心选择的移动应用架构

## 一个精心选择的架构**省去很多维护和开发的问题。**它也给出了开发过程和项目整体的结构。这在开发和维护项目的过程中非常重要，因为它帮助您改进或修复不存在的或糟糕的架构。这也是为什么我们相信**选择架构应该是每个软件项目规划阶段**不可或缺的一部分。

我们希望这些小技巧能帮助你的应用程序或项目进行[定制开发](/web/20220927122402/https://www.netguru.com/services/custom-software-development)，并为你节省大量时间。我们关心所有项目中精心挑选的架构，包括移动项目，因此如果您想与我们的专家讨论，请给我们留言。

We hope that these few tips will facilitate the [custom development](/web/20220927122402/https://www.netguru.com/services/custom-software-development) of your application or project and save you a lot of time. We care about well-selected architecture in all projects, including mobile ones, so if you’d like to discuss it with our experts, just drop us a message.