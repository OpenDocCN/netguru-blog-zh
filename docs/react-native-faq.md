# React Native 常见问题:所有你想知道但不敢问的关于 React Native 的问题(更新)

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/react-native-faq>

 关于选择 React Native 作为你下一个移动应用的框架，你还犹豫不决吗？我们会掩护你的。

我们选择并回答了当人们考虑在项目中使用 React Native 时最常见的问题。我们希望这个简短的指南能让事情变得更清楚。

## 1.什么是 React Native？

React Native 是一个框架，允许开发者使用 JavaScript 构建原生移动应用。它使你能够**使用相同的代码库为多个平台**构建应用程序。

这可以缩短开发时间并降低构建产品的总成本。另一个好处是，你不需要两个独立的 Android 和 iOS 团队，而是一个单独的 React 原生团队。

## 2.React Native 的重要性是什么？

React Native 最重要的一点是，你可以重用你为不同操作系统(iOS，Android)构建的代码。

这意味着在一个框架中为多个平台工作。这也是一个快速发展的框架，两年前，我们预测很快就可以使用 React Native 而不用接触任何本地代码，现在这比以往任何时候都更加真实。

## 3.React 和 React Native 有什么区别？

React Native 不是 React 的不同版本。反应本地使用反应。

本质上，React Native 是 React 的一个**定制渲染器，就像 ReactDOM 是 Web 的一样。除了转换 React 代码以在 iOS 和 Android 上工作，React Native 还提供了这些平台提供的功能。**

## 4.是不是每个 React 开发者也是 React 原生开发者？

在开发一个 [React 原生项目](http://web.archive.org/web/20221111121035/https://www.netguru.com/services/react-native-development)的大部分时间里，我们编写 React 代码，这些代码与 React for Web 中已知的代码没有什么不同，但是我们应该意识到移动平台的独特属性——有些事情可能不是那么明显。

默认情况下，React 开发人员不是 React 本地开发人员。也就是说，我们可以让 React 开发人员有机会获得 React Native 方面的经验，从而在短时间内将他们转变为 React Native 开发人员。

## 5.我的 React 应用程序能在移动设备上运行吗？我的 React 原生应用能在网络上运行吗？

可惜没有。

大多数 React Web 代码依赖于 Web 浏览器中可用的功能，因此它无法在移动平台上工作，反之亦然——React 原生**代码依赖于给定移动平台上可用的功能。**好消息是，我们仍然可以在移动和 web 应用程序之间重用一些代码，重用代码的能力将在未来得到提高。

## 6.在 ReactJS/PhoneGap 中构建移动 app 比使用 React Native 有什么优缺点？

首先值得一提的是，PhoneGap 在几年前是一个非常好的解决方案。如今，随着技术的变化，它已经不再适合大多数用户的需求。

PhoneGap 应用是通过“网络视图”嵌入移动应用的混合网站。您可以使用 ReactJS 完成类似的事情，甚至与 PhoneGap 结合使用，但是在这两种情况下，对本机和特定于设备的功能的访问都受到严重限制。 [React Native 的最大好处是性能比混合应用](http://web.archive.org/web/20221111121035/https://www.netguru.com/services/react-native-development)好得多。

## 7.为什么有些开发者觉得 Flutter 比 React Native 好？

最重要的是，[颤振是市场上的新事物](/web/20221111121035/https://www.netguru.com/services/flutter-app-development)。它是 Google 大力推广的，所以有非常好的支持。我们也可以想象，随着时间的推移，这种现代化的解决方案会越来越受欢迎。

总的来说，作为一个反应式框架，Flutter 具有**卓越的性能**并且易于学习，让你构建看起来像本地的应用程序，并且提供热重载。

尽管如此，React Native 仍然是跨平台移动开发的行业标准。通过使用比 Dart(Flutter 使用的语言)流行得多的 JavaScript，开发者可以更容易地采用它。

## 8.React Native 会让我的应用在 iOS 和 Android 上看起来和运行起来一样吗？

iOS 和 Android 提供不同的功能集，使这些环境平等不是 React Native 的责任。React Native 只是 iOS 和 Android 中访问原生组件的一种方式。

我要说的是，大多数时候——通过一些努力——我们可以让两个平台上的应用看起来一样，但我们不应该这样。谈到用户界面，我们应该**坚持平台指南**。幸运的是，React Native 为我们提供了一种简单的方法来使 UI 适应给定平台的需求。

**亦见:** [**反应本地人的利弊**](/web/20221111121035/https://www.netguru.com/blog/react-native-pros-and-cons)

## 9.我应该选择 React Native 还是 Native(分开 iOS 和 Android)？

对于大多数严重依赖用户界面的应用来说,React Native 非常棒，因为只需很少的努力，我们就可以让用户界面在 iOS 和 Android 上都能工作，最重要的是，我们可以**共享业务逻辑。**

除此之外，React Native 使用 flexbox 进行布局，这在 iOS、Android 和 Web 上的工作方式是一样的，所以我们可以从 Web 上转移我们的经验，而不是学习更多不同的引擎。

另一方面，当我们考虑到**使用平台提供的所有功能**，包括视频/音频处理或多线程等模块时，原生应用是非常棒的。由于 React Native 只关注用户界面，因此对于具有许多本机特性的应用程序来说，它的效率可能会较低。

## 10.React Native 是否意味着我们不需要原生开发者？

不，还是需要原生开发者。

在配置 Bitrise 等移动持续集成服务、HockeyApp 等内部应用分发服务、XCode 中的证书配置等方面，他们拥有丰富的经验。除此之外，

React Native 并不容易提供 iOS 和 Android 上可用的所有功能，本地开发人员可以通过创建一个自定义模块将这些功能暴露给 JavaScript 来提供这些功能。

当 React Native 还是一个新事物时，这曾经是一个大问题，但是现在你应该能够找到你能想到的大多数原生特性的库。很少需要编写自定义的本机模块。

React 原生应用还是有一些部分是用原生代码写的，这个没问题。

> *“React Native 并不意味着我们应该使用 Javascript 做任何事情，React Native 并不意味着我们不应该使用 Native 做任何事情”* Leland Richardson，Airbnb，React Conf 2017

未来是光明的，它属于反动派和反动派。；)

*本文首次发表于**2017 年 6 月 2 日*