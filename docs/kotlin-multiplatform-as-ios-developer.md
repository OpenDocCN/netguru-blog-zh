# 作为 iOS 开发者的 Kotlin 多平台

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/kotlin-multiplatform-as-ios-developer>

 关于“跨平台还是原生”的争论由于谷歌的骚动而愈演愈烈。谷歌正试图革新移动开发，与此同时，JetBrains 正在慢慢尝试开发一些“值得考虑”的解决方案。说实话，Java 很受欢迎...非常受欢迎，在未来，Kotlin 可能是下一个大事件，因为受欢迎程度预计会像现在这样增长。根据 [Stack Overflow 的 2018 年度开发者调查](http://web.archive.org/web/20220930111141/https://insights.stackoverflow.com/survey/2018/) Kotlin 是开发者中第二受喜爱的语言。

## Kotlin 多平台？

Kotlin 多平台将 JetBrains 完成的所有艰苦工作整合在一起，允许将 Kotlin 编译成三个不同的目标:JS、JVM 和 Native 是三个独立的项目，您也可以单独使用。Kotlin Native 就是其中之一，它基本上是一个将 Kotlin 代码编译成本地二进制文件的解决方案。主要是它被设计成允许在没有任何虚拟机的情况下编译。此刻[的目标平台](http://web.archive.org/web/20220930111141/https://kotlinlang.org/docs/tutorials/native/targeting-multiple-platforms.html)是:

*   ios
*   马科斯
*   机器人
*   Windows 操作系统
*   Linux 操作系统
*   web 程序集

这意味着我们的业务逻辑只需编写一次，就可以在所有这些平台上本地共享。不错！

## **跨平台**

当决定是本地开发还是跨平台开发时，有几件事我们必须考虑。首先是金钱和时间——从商业角度来看，这些是最重要的东西。众所周知，创建跨平台应用程序更便宜、更快。在平台之间共享代码似乎是一个好主意，但是……它总是这么好吗？

第二件事来了——**技术**。

我们有 Xamarin，它编译成 CIL 和 React Native，基本上是 JS。有了它们，我们在 UI 层之上有了额外的抽象。我们还运行了一个在给定平台上性能可能不是最好的代码。JS 将必须在 V8/JavaScriptCore 上运行，然后通过某种桥梁与本机代码通信。同样的情况也会发生在 Xamarin 上，它将使用 ACW 和 MCW 来包装 Android 上的本地调用(类似于 JNI 的机制)。

Kotlin 多平台编译到多个目标，在我们需要的地方给我们本地二进制文件——iOS 或 JVM 字节码输出——例如 Android。

此外，我们使用为 UI 构建而制作的标准组件，因此我们可以完全分离和区分两个平台的 UI。

让我们以我们的 Kotlin 多平台存储为例(由 Kotlin 多平台制造)。你可以在这里 阅读更多关于创建它的信息[](/web/20220930111141/https://www.netguru.com/blog/what-weve-learned-by-developing-the-kotlin-multiplatform-storage-library)

 我们想为这两个平台创建安全的钥匙链包装器，所以我猜 *UserDefaults* 不是这种数据的最佳位置。首先，我们试图实现完全用 Kotlin 编写的*钥匙链*，但由于缺乏 Objective-C 的角色转换，这最终似乎是不可能的。经过多次尝试，我们最终决定，我们将使用 c-interop 工具，创建原生库，并将其连接到 Kotlin 应用程序。我们所做的，基本上是创建 Objective-C 库，该库将从链接框架创建 Kotlin 接口，因此我们可以在 Swift 应用程序中使用它！是的…我知道-跨平台...

现在很难说这些措施中是否有可以做得更好的。这项技术仍然是非常实验性的，没有文档，没有教程，没有堆栈溢出主题。

## Kotlin 多平台背后的基础是什么？

我们有两种类型的模块——特定于平台的和通用的。

特定于平台的模块定义视图，并从公共视图实现代码。

**公共主模块**是保存项目所有公共部分的模块。放在这个模块中的代码在每个声明的平台之间共享。但是，它有一些限制，因为您不能在这里访问特定于平台的特性。相反，**公共主**模块可以使用[期望/实际机制为那些特性定义一个公共接口。](http://web.archive.org/web/20220930111141/https://kotlinlang.org/docs/reference/platform-specific-declarations.html)所以，这是实现通用业务逻辑的好地方！

**平台包(**例如**【安卓主】)**是依赖于平台的代码。例如平台的框架实现。

## **实施**

这是一个 iOS 平台的模块示例

仅仅为了利用共享代码库，您必须在您的 Swift 类中实现接口。在本例中，我们将拥有*主视图*界面

Kotlin

```
interface MainView {

    fun showElementAddedInfo()

    fun showToDoList(toDoList: List<String>)
} 
```

迅速发生的

```
final class MainViewController: UIViewController, MainView { }
```

接口为您提供了在 Kotlin 中使用的方法。这就是斯威夫特“委托”科特林方法的方式。以及它是如何以另一种方式运作的？

我们只需要创建用 Kotlin 编写的 *Presenter* 的实例，这样我们就可以使用那里创建的方法。

Kotlin

```
class MainPresenter {
    fun foo() {
        print(„bar")
    }
} 
```

Swift

```
private lazy var presenter: MainPresenter = {
    MainPresenter()
}()

func bar() {
    presenter.foo()
} 
```

另一件事，我们不能忘记的是附加和分离视图

我们应该在创建 *ViewController* 实例时附加视图。没有它，Kotlin 代码就不能被正确调用。

```
presenter.attachView(view: self)
```

attachView 正在创建对传递的对象的强引用，所以要释放对象，我们只需调用

```
deinit {
    presenter.detachView()
} 
```

我的锅

```
fun detachView() {
    this.view = null
} 
```

毕竟，我们不需要太多来利用共享代码库，但是上面我们有一个场景，当整个环境已经设置好了。因此，您必须导入生成的框架，在构建设置中更改框架搜索路径，并调用脚本。如果你想试一试，你应该看看这个

## **结论**

作为 iOS 开发者，你可以毫无问题地阅读 Kotlin 代码。较小代码片段的语法相似度可以超过 60%，所以在 Kotlin 中读写代码不成问题(甚至有现成的转换器可以使用，比如这个 [one](http://web.archive.org/web/20220930111141/https://github.com/angelolloqui/SwiftKotlin) )。如果没有代码片段上面的注释，可能很难判断哪个代码是用 Swift 或 Kotlin 编写的。但令人难过的是，您仍然需要知道如何在其他平台上创建视图，因此至少必须了解特定于平台的 UI 构建。此外，像 Kotlin Native 这样的解决方案可以成为未来，一个代码库，许多目标，包括后端，前端，移动，甚至嵌入式！现在这项技术还处于实验阶段，还没有准备好投入生产，但是我们很可能会在未来听到更多关于它的消息，因为它有很大的潜力。