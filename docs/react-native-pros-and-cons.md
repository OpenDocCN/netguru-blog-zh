# 反应本土的利弊——脸书 2021 年的框架(更新)

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/react-native-pros-and-cons>

 当我们第一次听说 React Native 这个支持多平台移动应用开发的框架时，我们激动不已。一个团队，一个代码库，以及利用本地[移动应用开发](http://web.archive.org/web/20221201175534/https://www.netguru.com/services/mobile-development)所需的资源来扩展 iOS 和 Android 应用的潜力是一个伟大的承诺。但是这和现实有什么关系呢？

* * *

Netguru 制造数字产品，让人们以不同的方式做事。与我们的团队分享您的挑战，我们将与您合作，推出革命性的数字产品。用 Netguru 创建一个产品。

* * *

两年前，我们决定赌一把，在 React Native 还相当不稳定的时候投资它。现在，有三个团队，总共有 14 名成员，我们确信 RN 会留在这里。我们还运行了两个版本的 RN academy 来分享我们的知识，并鼓励其他人尝试 RN 开发。我们也想以博客的形式分享我们的见解和经验。这是我们对 React Native 的最新更新。

直到几年前，如果我们瞄准两个市场，单独的 iOS 和 Android 应用程序开发是必要的。拥有两个不同的团队——每个系统一个团队——会产生更多的成本，尤其是当我们考虑持续升级和定期引入新功能时。

几年前，[脸书正式宣布推出 React Native，这是一个强大的框架，有望实现跨平台兼容性](/web/20221201175534/https://www.netguru.com/blog/why-react-native-mobile-app)。RN 自推出以来已经成熟，我们有机会在战斗中测试它。我们已经为商业和内部项目构建了许多应用程序，现在我们可以权衡 React Native 的利弊。真的值得你关注吗？

React Native 的优点

![React Native pros and cons by Netguru](img/c14e53b29e8ebde5b9422dd572afedd2.png)

1.构建速度更快

## React Native 的主要卖点是[更短的开发时间](/web/20221201175534/https://www.netguru.com/blog/react-native-faq)。该框架提供了许多可以加速这一过程的现成组件。

### React Native 仍然缺乏一些解决方案，所以您可能需要从头开始构建它们，但是 RN 是基于 JavaScript 的，它提供了对世界上最大的包生态系统的访问。拥有如此广泛的软件包，你可以节省很多时间，而且只会变得更好。

随着 RN 社区的成长，脸书定期推出新的更新，我们可能会在某一天找到我们需要的大多数解决方案的现成组件。

我们用 React Native 和 Swift 构建了完全相同的应用程序。后者花费了多 33%的时间来构建 T1，并且仍然只能在 iOS 上运行，没有 Android 版本。为多个平台开发可以节省大量成本。由于 RN 允许您在几个小时内在操作系统之间共享很大一部分代码库，您可以潜在地节省时间和金钱。

2.一个框架，多个平台

React Native 允许你在 iOS 和 Android 之间重用代码库(或者只是其中的一部分)。实际上，[完全跨平台开发在某种程度上是可能的](/web/20221201175534/https://www.netguru.com/blog/cross-platform-mobile-apps-development)，这取决于您在应用程序中使用多少原生模块。

### 一些特性将在 npm 包中提供，但其他特性需要从头开始编写。不过，情况只会越来越好。React Native 社区积极支持该框架，为其开源添加新工具。

使用 JavaScript 开发还提供了一个机会，不仅可以在移动平台之间共享代码库，还可以在 React web 应用程序之间共享代码库。像 React Native Web 这样的工具的积极开发似乎指向了这个方向。由于技术非常相似，这也允许相同的开发人员在 web 和移动应用程序上工作。

尽管这种解决方案的稳定性仍有许多不足之处，但共享非 UI 依赖代码的可能性仍可能带来合理的好处。除了缩短开发时间，它还可以帮助提高所有支持平台之间应用程序业务逻辑的一致性。

快速刷新

React Native 团队似乎在认真倾听社区的声音。React 本机的一个最大问题是热重新加载功能被破坏。大多数开发者关闭了它，因为它不可靠。对这些抱怨的回应是快速更新。这个工具，[于 2019 年底](http://web.archive.org/web/20221201175534/https://reactnative.dev/blog/2019/09/18/version-0.61#fast-refresh)发布，修复了热重装存在的所有问题，并提供了出色的开发者体验，加快了构建新功能和修复 bug 的速度。

### 4.较小的团队

原生开发需要 Android 和 iOS 两个独立的团队。它会妨碍开发人员之间的交流，并因此减慢开发速度。原生 Android 和 iOS 团队都有自己的项目，他们往往有不同的开发速度和方法。因此，两个项目很容易变得不一致。这可能会导致应用程序实现的功能、行为和外观的差异。

### 如果您选择 React Native，您将主要需要 JavaScript 开发人员，他们可以为这两个平台编写代码。显然，具有更多本地特性的应用也需要本地开发者的更多帮助。也就是说，在大多数情况下，团队的规模会更小，因此更容易管理。

5.快速应用

许多人认为 React 本机代码可能会阻碍应用程序的性能。JavaScript 不会像本地代码一样快，但是在大多数情况下，你看不到差别。

### 我们运行了一个测试，并比较了用 React Native 和 Swift 编写的简单应用程序的两个版本。两个应用取得了非常相似的性能结果。性能差异很小，一般用户几乎察觉不到。在更复杂的应用程序的[情况下，框架可能效率较低，但是您总是可以将一些代码转移到本机模块，这将不再是一个问题。](/web/20221201175534/https://www.netguru.com/blog/what-are-single-page-applications)

6.简化用户界面

React Native 以创建移动 UI 为坚实基础。在本地开发中，您需要在应用程序中创建一系列操作。RN 采用声明式编程，这种实现动作的顺序已经过时。因此，在用户可以选择的路径上发现错误要容易得多。

### 7.大型开发者社区

React Native 是一个开源的 JavaScript 平台，允许开发人员将他们的知识贡献给框架的开发，所有人都可以免费访问。如果开发人员遇到问题，他们可以向[社区寻求支持](http://web.archive.org/web/20221201175534/https://www.netguru.com/services/react-native-development)(截至 2020 年中期，有近 50，000 名活跃的贡献者参与了堆栈溢出时的 React Native 标记)。总会有人能够帮助他们解决问题——这对提高编码技能也有积极的影响。

### 反应原生的缺点

1.兼容性和调试问题

## 虽然这可能令人惊讶——毕竟，React Native 是由顶级技术玩家使用的——但它仍处于测试阶段。您的开发人员可能会遇到各种关于包兼容性或调试工具的问题。如果您的开发人员不精通 React Native，这可能会对您的开发产生负面影响，因为他们会花时间进行故障排除。

### 2.缺少一些自定义模块

尽管已经成熟，React Native 仍然缺少一些组件。反过来，其他国家也不发达。您很可能对此没有问题，因为您需要的大多数定制模块都是可用的，有良好的文档记录，并且工作正常。

### 然而，可能发生的情况是，您将不得不从头开始构建您的解决方案，或者尝试破解一个现有的解决方案。在开发定制模块时，你可能会为一个组件找到三个代码库(RN、Android 和 iOS ),而不是只有一个。在每一个代码库中，行为和外观都可能存在差异。幸运的是，这种情况不常发生。

我们在让阴影在我们的 React Native 应用程序中工作时遇到了问题，因为自定义库只在测试版中可用。因此，我们决定编写自己的自定义解决方案，使其看起来与本机应用程序中的一样。

3.仍然需要本地开发人员

实现一些本机特性和模块需要对特定平台有详细的了解。许多原生应用功能(例如推送通知)缺乏开箱即用的支持[曾经是](/web/20221201175534/https://www.netguru.com/blog/why-native-app-development) [React 原生开发](http://web.archive.org/web/20221201175534/https://www.netguru.com/services/react-native-development)的一个重大问题。

### 随着社区的发展，越来越多的开源库提供了对原生平台特性的便捷访问。然而，一些更高级功能的实现可能仍然需要 iOS 和 Android 开发者的帮助。

他们的输入取决于你的项目的复杂性，但是你需要在开始使用 React Native 时为他们讨价还价。对于小团队来说，这可能是一个问题，因为开发人员没有任何本地移动体验。

替代反应原生

1.当地的

## React Native 最明显的替代方案是针对原生 iOS 和 Android 平台的单独开发。如前所述，这涉及到两个团队开发两个不同的应用程序。这可能会导致应用程序变得不一致，并且您将无法重用代码。

### 如果你想让你的应用程序真正“有本土的感觉”或旨在实现最大可能的性能，本土应用程序可能仍然是最好的方法。更好的文档和对流行问题的现成解决方案的更广泛的可用性是一些其他的优势。但是要意识到成本和时间的影响。

2.摆动

目前，React Native 最受欢迎的跨平台替代品是 Flutter，这是谷歌开发的一个相对较新的移动应用框架。

### 它基于 Dart 编程语言，采用不同的方法来实现跨平台特性。在 Netguru，我们喜欢关注新兴技术，所以我们做了一些关于 Flutter 的研究。

我们发现它可以为借助框架开发的应用程序提供出色的性能和本机外观。由于 Flutter 是一个相当新的事物，所以它的群体要比 RN 小得多。此外，请记住，对于[前端开发人员](/web/20221201175534/https://www.netguru.com/services/front-end-development)来说，切换到 React Native(尤其是那些有一些 React 经验的人)比学习一门全新的语言和框架更容易。不过，对于 Java 或 C#开发人员来说，Dart 可能更容易一些。

3.其他的

跨平台移动开发还有许多其他选择。NativeScript 是一个框架，允许使用 Angular 或 Vue.js 等 web 框架开发移动应用程序。Xamarin 使用 C#语言，像 RN 一样，它将代码编译成本地控件。

### Ionic 基于 WebView 中的渲染应用程序，这可能比其他方法慢。一个简单应用程序的有趣替代方案可能是开发一个[渐进式网络应用程序](/web/20221201175534/https://www.netguru.com/blog/what-is-a-progressive-web-app-and-when-you-should-go-for-it)，一种适合移动和离线使用的特殊类型的网站。

反应本土经验——别人怎么说

React Native 已经被很多人采用，效果有好有坏。总体反馈是积极的，大多数人认为注册护士是一个很好的机会，并坚持下去。

## [Discord 对 iOS 使用 React Native 非常满意](http://web.archive.org/web/20221201175534/https://discord.com/blog)。有趣的是，他们决定保留 Android 应用程序的原生版本。他们声称注册护士允许他们保持团队的小规模和高效率。至于最大的痛点，他们提到了一些性能问题，例如处理非常长的列表，以及他们有时仍然需要一些本机平台知识的事实。

2020 年初，跨国电子商务公司 Shopify 发表了一篇博文，标题意味深长:React Native 是 Shopify 的移动未来。这对脸书的框架来说是个好消息，因为 Shopify 可以为开源解决方案做出巨大贡献。

微软可能是 React 原生社区中最有影响力的玩家之一。他们与脸书合作，在 React Native 上投入了越来越多的资金。由于他们的努力，React Native 支持 Windows 和 macOS 应用程序。

投资该技术的最大公司之一，即 Airbnb，[决定淘汰 React Native](http://web.archive.org/web/20221201175534/https://medium.com/airbnb-engineering/sunsetting-react-native-1868ba28e30a) 。这一决定是由多种技术和组织问题推动的，主要是由于项目的巨大规模以及将 RN 纳入现有原生应用的必要性。

UberEATS 分享了[一份非常详细的报告](http://web.archive.org/web/20221201175534/https://eng.uber.com/ubereats-react-native/)关于他们迁移到 React Native 的过程。代码重用的可能性超出了他们最初的预期，总体体验被描述为非常积极。

在 Artsy，迁移到 RN 完全改变了开发文化和结构，因为他们不再有专门的 iOS 开发团队。他们指出，要在 RN 上取得成功，一些本地经验是必要的，但你不需要雇佣一个专门的本地专家团队。

总结

在开始使用最流行的跨平台移动开发框架 React Native 之前，上述优点和缺点值得考虑。

## RN 的优势包括在较小的团队中更快的开发，跨多个平台重用代码库的可能性，以及为开发人员提供一些方便的工具(比如快速刷新)。

请注意，该框架仍然存在一些问题，但它们大多与技术的不成熟有关，并且很可能在未来变得不那么麻烦。

在从事更大或更复杂的项目时，你必须记住，来自原生 Android 和 iOS 开发人员的一些帮助可能仍然是必要的。

总的来说，我们看到了 React Native 的光明前景——它仍处于非常活跃的开发阶段，社区也在不断发展。要全面了解该框架，请查看我们的[终极常见问题列表](/web/20221201175534/https://www.netguru.com/blog/react-native-faq)或直接联系我们的团队。

While working on bigger or more complex projects, you have to keep in mind that some help from native Android and iOS developers might still be necessary.

Overall, we see the future of React Native in bright colors – it is still under very active development, and the community keeps on growing. To get a full picture of the framework, check out our [ultimate FAQ list](/web/20221201175534/https://www.netguru.com/blog/react-native-faq) or contact our team directly.