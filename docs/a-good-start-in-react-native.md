# React Native 的良好开端

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/a-good-start-in-react-native>

 如果你正在阅读这篇文章，你可能属于以下两类人之一:

您是一名原生 iOS 或 Android 开发人员，正在考虑过渡到 RN，或者

你对 rn 已经很熟悉了，但是有个朋友，是原生 iOS/Android 开发人员，向你请教如何学习 RN。

在这两种情况下，你都是受欢迎的，这篇文章是给你的！

半年前，我和 *JS* 毫无共同之处，但我成功地完成了转变。现在，我可以分享一些提示，可以**把你从我犯的几个错误中拯救出来。这篇文章并不深入细节，而是**对**到 *RN* 的一个简短而宽泛的介绍。我还提供了更多详细资源的链接。**

!["Hello World" app code](img/ced516ee5f68dd51c1b3d8df3a1a9b3b.png)

只是一个“Hello World”应用程序的例子

## 动机

首先，让我告诉你一些背景，为什么我决定改变我选择的关于移动应用程序开发的技术。我有本地开发( *iOS* 和 *Android* )以及使用 *Xamarin* 和 *Unity* 进行跨平台开发的经验。我喜欢所有这些技术，但我在寻找应用程序开发的新东西，这些东西可以让我结合我已经知道的东西。我相信跨平台开发肯定是正确的方向。我选择了解 *React Native* 。

第一次用的时候**完全搞不清楚**。它不像我以前用过的任何东西。不同的语言，不同的环境，一个范式的转变。**花了我一些时间**来适应新技术，但现在我确信我做出了正确的选择来过渡到注册护士。

如果你害怕*JavaScript*——不要害怕。过去我听到很多关于 *JavaScript* 的负面意见，但是现代标准( *ES6* ) **已经变得更好了**。JS 也是最受欢迎的语言，因此它的社区很大。在 *npm* 中也有许多高质量的包可以在 *RN* 应用中重用。

## 什么是反应原生

重要的事情先来。[什么是 React Native](/web/20220927120740/https://www.netguru.com/glossary/react-native) ？你可能已经知道这是一个基于 *React* 的框架，用于使用 *JavaScript* 开发**跨平台应用**。 *RN* 使用了与 *React.js* (也叫 *React Web* )不同的渲染机制。*组件*，即**一个 app 的主要构件**，被翻译成**原生移动视图** : *UIView* 在 *iOS* 或 *View* 在 *Android* 上。

使用 *JavaScript* 、 *HTML* 和 *CSS* 的跨平台框架**经常被指责**没有原生的感觉和/或遭受许多性能问题。虽然这通常适用于在 *WebView* 中呈现视图的框架，但它**不会将**应用到 *React Native* 。

对于我们这些本地开发者来说，理解*中的*本地*代表什么是【反应本地*尤为重要。它代表了**原生视图**的渲染，这是一件好事。一个不好的地方是它仍然运行不那么本地的 *JS* ，而不是更快的本地代码。幸运的是，通常很难发现**性能差异**。
如果要深究细节，我们**对比了两个一模一样的 app**，一个用 *Swift* 写的，另一个用 *RN* ，[写了一个关于它的故事](http://web.archive.org/web/20220927120740/https://www.netguru.com/blog/swift-vs-react-native)。

## 如何开始

### 指导

当我在*迈出**第一步**时，我读了很多指南，我可以**特别推荐**这两个:* 

*   脸书的官方 *React Native* 【入门】指南是一个非常棒的入门资源。它是最新的和有条理的。在本指南的最开始，你必须**选择一个工具链** : *React Native CLI* 或 *Expo* 。但是，我劝你长期不要用 *Expo* 。 *Expo* 在最开始时**比较容易，但后来**就变得麻烦了**，因为并不是所有的依赖项和本机模块都能很好地与之配合。作为一个本地开发者，你应该考虑一下重用你从本地世界已经知道的东西的可能性，而不要在 T21 世博会上限制你的选择。幸运的是，你总是可以从 *Expo* 中*弹出*你的代码到一个**纯*RN*项目中，这个项目将与标准 *React Native CLI* 一起工作。当使用 *React Native CLI* 时，您将**了解更多**，并且您的项目将不会受到 *Expo 的*限制。T39****
*   **另一个好的向导是[【React Native Express】](http://web.archive.org/web/20220927120740/http://www.reactnativeexpress.com/)**。这个是互动的。它从 JavaScript 的基础开始，然后进入更高级的主题，如 *Redux* 和*动画 API* 。  

如果你**渴望更多的知识**，我们也写了一个更广泛的工具和资源选择，被**认为是我的同事们最有用的**。在[这篇 Codestories 博客文章](/web/20220927120740/https://www.netguru.com/blog/learn-react-native)中可以找到。

### 工具

你可以使用**任何你喜欢的文本编辑器或 IDE** 编写 *RN* 应用程序。目前在*RN*dev 中最流行的工具是 [*Visual Studio Code*](http://web.archive.org/web/20220927120740/https://code.visualstudio.com/) 。它是高度**可定制的**，并提供对大量**扩展**的访问。另一个值得尝试的流行 IDE 是 [*Deco*](http://web.archive.org/web/20220927120740/https://www.decoide.org/) 。只要你觉得**更适合你就行**。

当涉及到**调试**的时候，你可以只使用你的浏览器( *Chrome DevTools* ，但是还有**更强大的替代品**可用。其中之一是 *React 原生调试器*，它作为一个独立的应用程序运行。除了一个必备的调试器控制台，它还为您提供了工具来轻松地**分析应用程序的存储状态**或查看**组件的层次结构**。

虽然它是一个很棒的调试器，通常可以节省很多时间，但不幸的是，它有一个小小的缺点:它大大降低了应用程序的运行速度。我给你的提示是，当你真的必须调试**一些非平凡的**时，使用它，否则使用浏览器中的默认调试器。

我们已经发布了一篇博文，更详细地介绍了调试 T1 的细节。在那里，你可以读到关于**的另一个强大工具**—*反应堆*。它的调试方法略有不同，而且**比 *React 本机调试器*运行得更快。**

万一遇到最复杂的问题，你仍然可以使用你已经熟悉的 T2 Xcode 和 T4 Android Studio 进行分析和调试。当你需要**剖析性能**或**解决奇怪的渲染问题**时，它们是最好的。

!["Hello World" app preview](img/ae869bf8bbb0169bba0bae3787509a80.png)

它真的**呈现原生视图**。上面的截图，以防你不相信我，显示了 **[在 *Xcode* 中捕获的视图层次](http://web.archive.org/web/20220927120740/https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/debugging_with_xcode/chapters/special_debugging_workflows.html)** 。 *Android Studio* 提供 **[类似功能](http://web.archive.org/web/20220927120740/https://developer.android.com/studio/debug/layout-inspector)** 。

### 航行

一个新的 *RN* 项目的第一步将是选择一个导航库。选择并不简单，因为有**无官方** *RN* 导航和许多图书馆在竞争。这两个我认为是最常用的**:**

 ***   最受欢迎的库大概就是[*react——导航*](http://web.archive.org/web/20220927120740/https://reactnavigation.org/) 。它被认为是**相对容易的**，但是**不是最有性能的**，因为它没有显示本地导航组件。
*   另一个流行的是*react-native-navigation*(wix 创造)。如果你有**本地背景**，你绝对应该试试这个库，因为它更加**可定制**，并且使用**真正本地的**导航组件。

## 调用本机代码和性能

基本上，在 *RN* apps 代码可以拆分成**两个世界** : *JS* 和原生。是的，显然你可以**从 *JS* 调用原生代码**。 *React Native* ，通过*桥*，将 *JS* 接口暴露给本机平台的 API。这意味着该应用程序可以使用**原生功能**，如摄像头、蓝牙和 *GPS* 。

分别地，这些 JS 和本地世界中的代码运行**相当快**，但是有时它们之间需要**通信**，不幸的是这很慢。这座*桥*在两个世界之间充当**翻译器**。

![Bridge between JS and native code](img/6d1ebd12ffcdd9738ebf4797ba22bbd8.png)

图示了 JS 和本机代码之间的*桥*

这个概念的一个**有趣的例子**是标准列表结构( *FlatList* )是如何在 *RN* 中实现的，因为它与我最初的预期不同。 *FlatList* 不重用一个原生类，比如 *Android* 上的 *RecyclerView* 或者 *iOS* 上的 *UITableView* ，而是通过*桥*使用 *JavaScript* 对**限制调用**从零开始实现**。当这种交流经常发生时，纯粹用 JS 编写的代码会比在桥后面的本机端运行逻辑的代码运行得更快。**

知道了这一点，你应该试着将通过桥的**调用的数量限制到最小。官方文档(iOS，Android)也是解释**原生模块**细节的相当不错的资源。**

通常你不必深入到原生世界，因为你需要的这个独特的原生特性很可能已经被 *npm* 中的某个包**实现了**。正如我之前所说的， *JS* 和 *React Native* 社区是**庞大的**和**活跃的**，所以很可能已经完成了。

## 生命周期

在我与 *RN* 的冒险之初，我很难理解*组件*的生命周期。这可能是因为我期望它**看起来像本地生命周期**。嗯，正如你所猜测的，这是**非常不同的**。事实上，在 *RN* 中，我们的视图有**两个生命周期**:iOS*上的 *UIViewController* 或安卓*上的 *Activity* 封装了*组件*的生命周期。**

![React Native Component lifecycle](img/e9337597382f7aba976ff16aa806849b.png)

展示本地生命周期中 *RN 组件*生命周期的各个阶段的图表。

我们继续来看 *iOS* 上 *UIViewController* 的**实例**和 *Android* 上*活动*。他们的生命周期跨越了他们的一生，就像一部电影从开始到结束一样。这与*组件*不同，其*更新*阶段生命周期看起来更像是**电影**中的单帧。

![Rhe detailed lifecycle of React.Component](img/ef93a700e1274ee6757d01f4d009d1b2.png)

图表显示了 *React 的详细生命周期。组件* ( [来源](http://web.archive.org/web/20220927120740/http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/))

*组件*的*挂载*阶段在**只完成一次**按照其包装原生视图的寿命。在*componentidmount*调用之后，我们知道*组件*被绑定到一个本地视图，并且**首先使用初始*属性*和*状态*进行渲染**。这里有一些解释[道具](http://web.archive.org/web/20220927120740/https://reactjs.org/docs/components-and-props.html)和[状态](http://web.archive.org/web/20220927120740/https://reactjs.org/docs/state-and-lifecycle.html)的官方文档的链接。

下一阶段是*更新*。这是我与《T2》相比的第一阶段，一个单独的电影画面。*组件*每次接收到新的*道具*或其*状态*发生变化时，都会经历这个阶段**，这些是**可以重新渲染的唯一条件**。在**每次更新**之后，呈现新的电影帧。**

在*组件将卸载*调用之后的*卸载*阶段，我们知道*组件*将很快从本机视图中**分离**。

值得注意的是，在学习时，你可能会找到介绍*组件*的**遗留生命周期**的资源。请记住，*组件将安装*，*组件将接收 Props* 和*组件将更新*被视为**过时**，因为*反应*v 16 . 3 . 0(2018 年 3 月 29 日发布)。

## 体系结构

对我来说，开发一个完全原生的应用程序和一个 *RN* 应用程序之间最明显的区别是**完全不同的架构**。原生的 *iOS* 和 *Android* 项目通常使用一种流行的**架构模式** : *模型-视图-东西*，*毒蛇*或类似的模式来编写。
在本地开发中最常用的架构模式通常不适合*的反应。组件* **生命周期**。这就是为什么**状态容器**被用作*冗余*在 *RN* 中如此流行的原因。值得注意的是，在《原生世界》中， *MVI* 范式正受到越来越多的关注。事实上， *MVI* 实现了*通量*概念，所以它**与大多数 RN 应用架构**非常相似。

![The most popular architecture for React Native apps - Redux](img/fadd98be8d1aba6ebb20f1e91f5ac273.png)

图解说明最受欢迎的 *RN* 应用架构- Redux

由于 *RN* 是基于 *React.js* 的，它也使用了**的很多工具**。最值得注意的部分是对**状态管理库**的使用，其中最流行的是 *Redux* 。没有它也可以编写 *React* 应用，但是如果你想成为 *RN* dev **你真的应该了解 *Redux*** 。如果你想要一个**详细的答案** [为什么 *Redux* 对一个 *RN* app](http://web.archive.org/web/20220927120740/https://blog.logrocket.com/why-use-redux-reasons-with-clear-examples-d21bffd5835) 有好处，就看这篇文章。

## 摘要

这个 *RN* 环境可能看起来与你从本地 *iOS* 或 *Android* 所了解的完全不同，但是我希望有了这个指南**过渡**会更容易**。你的职业和满足感可以从学习新东西中获益良多，尤其是如果你对当前的技术感到厌倦或者只是在寻找新的挑战。**

 **你觉得这篇文章有帮助吗？如果你喜欢或与朋友分享，我会**非常高兴**。

RN 与你最初的预期有什么不同？你想要我们详细描述某事吗？欢迎**在评论区写下你的想法**。

**祝你好运 *React Native* 冒险！我相信你会喜欢的！**

* * *

照片由 [吉安塞斯孔](http://web.archive.org/web/20220927120740/https://unsplash.com/photos/N0g-deioHO4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [下](http://web.archive.org/web/20220927120740/https://unsplash.com/search/photos/phones?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)****