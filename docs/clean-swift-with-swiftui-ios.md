# 使用 SwiftUI 清洁 Swift iOS 架构设计

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/clean-swift-with-swiftui-ios>

 自 2019 年 9 月 SwiftUI 框架发布以来，大多数 iOS 应用程序开发人员要么等待其他人找出一种以干净的方式使用新 UI 库的方法，要么自己尝试。

在这里，我想分享一下我为自己从事的商业项目准备的方法。我想继续 UIKit 中应用的导航流程，用干净的 Swift clean 架构保持业务逻辑的高可测试性，而尝试用 SwiftUI 实现视图。

## 为什么不改用纯 SwiftUI？

我在 Swift 中看到很多 Redux、MVVM 等知名的 UI 架构的架构设计模式的端口与 SwiftUI 一起使用。然而，他们都没有说服我，它可以用在一个相当旧的项目中，严重依赖于 Objective-C 中的旧组件(singletons 和 managers ),最重要的是,仍然需要从我们添加的新场景中推送或显示用 UIKit 编写的旧视图控制器。

除此之外，我试图保持项目在业务逻辑分离的情况下是可测试的，不切换到 pure SwiftUI 的另一个重要原因是 Clean Swift 已经被用作所有现代模块的主要模式。

如果你想知道 Clean Swift 架构设计模式是什么，[在阅读这篇文章之前先看看我以前的文章](/web/20221111121043/https://www.netguru.com/blog/clean-swift-ios-architecture-pattern#:~:text=Clean%20Swift%20(VIP)%20was%20first,in%20Uncle%20Bob'sClean%20Architecture.)。

## 有哪些变化？

将 SwiftUI 与 Clean Swift 一起使用需要做的主要改变是 :

*   *SceneViewController* 将保存一个对 *SceneViewModel* 的引用，它是我们*视图的数据、状态和代表的来源。*

    ```
    protocol WelcomeSceneViewControllerInput: AnyObject {
        var router: WelcomeSceneRoutingLogic? { get set }
        var viewModel: WelcomeSceneViewModel? { get set }
    }

    ```

*   *SceneViewModel* 是一个新类，我们可以使用 Combine 使 SwiftUI 的视图具有反应性。

    ```
    protocol WelcomeSceneViewDelegate: AnyObject {     func didSelectButton(_ sender: WelcomeSceneViewModel?) }  protocol WelcomeSceneViewModel {     var delegate: WelcomeSceneViewDelegate? { get set }     var text: String { get }     var buttonText: String { get } }  final class DefaultWelcomeSceneViewModel: WelcomeSceneViewModel {     var delegate: WelcomeSceneViewDelegate?     @Published private(set) var text: String     @Published private(set) var buttonText: String          init(         text: String,         buttonText: String     ) {         self.text = text         self.buttonText = buttonText     } } 
    ```

*   *场景视图控制器*继承自 *UIHostingController <场景视图>* ，而不是继承自 UIViewController。

    ```
    final class WelcomeSceneViewController: UIHostingController {     var interactor: WelcomeSceneViewControllerOutput?     var router: WelcomeSceneRoutingLogic?     var viewModel: WelcomeSceneViewModel? } 
    ```

    在这里查看我的示例项目[中的使用示例。](http://web.archive.org/web/20221111121043/http://welcomesceneviewcontroller.swift/)
*   *场景配置器*获取*视图模型*并将其分配给*视图控制器。* 将方法'配置好(...)'包含了许多样板代码，但只要您使用本报告中的[我的代码模板和为配置器模块提供的测试用例，这应该不是问题。

    ```
    protocol WelcomeSceneConfigurator {     func configured(         with viewModel: WelcomeSceneViewModel     ) -> WelcomeSceneViewController }  final class DefaultWelcomeSceneConfigurator: WelcomeSceneConfigurator {     func configured(         with viewModel: WelcomeSceneViewModel     ) -> WelcomeSceneViewController {
            var viewModel = viewModel         let viewController = WelcomeSceneViewController(             rootView: WelcomeSceneView(viewModel: viewModel)         )         let interactor = WelcomeSceneInteractor()         let presenter = WelcomeScenePresenter()         let router = WelcomeSceneRouter()         router.source = viewController         presenter.viewController = viewController         interactor.presenter = presenter         viewController.interactor = interactor         viewController.router = router         viewController.viewModel = viewModel         viewModel.delegate = viewController         return viewController     } } 
    ```](http://web.archive.org/web/20221111121043/https://github.com/strzempa/CleanSwiftWithSwiftUI/tree/main/XcodeTemplates/CleanSwift%20%2B%20SwiftUI) 
*   我们的 *ViewController* 的*视图*不再从 *UIView* 继承，而是从 SwiftUI 的*视图继承。* 

    ```
    struct WelcomeSceneView: View {     let viewModel: WelcomeSceneViewModel          var body: some View {         VStack {             Text(viewModel.text)             Divider()             Button(viewModel.buttonText) {                 viewModel.delegate?.didSelectButton(viewModel)             }         }     } } 
    ```

如果以上片段还不够，请查看我的 GitHub 账户中的示例项目[。](http://web.archive.org/web/20221111121043/https://github.com/strzempa/CleanSwiftWithSwiftUI)

## Clean Swift + SwiftUI 总结

#### 它给项目带来了什么？

*   用非常高的可测试性、可重用的组件实施模块化，并提取业务逻辑。
*   可应用于任何规模和年龄的现有项目，目标部署在 iOS 14 以上。

#### 带 SwiftUI 的 Clean Swift 的缺点

如果您在本文的任何地方感到困惑，可能是因为我假设您已经了解了用于 iOS 开发的 Clean Swift 架构设计模式。如果你不知道，或者只是想回顾一下基础知识，请查看[我之前的博客 Clean Swift (VIP) iOS 架构模式。](/web/20221111121043/https://www.netguru.com/blog/clean-swift-ios-architecture-pattern#:~:text=Clean%20Swift%20(VIP)%20was%20first,in%20Uncle%20Bob'sClean%20Architecture.)

我的想法只是众多想法中的一个，很难说混合 UIKit 和 SwiftUI 的方法会在这些年里有什么变化。拿着它，按照你的项目需要的方式修改它。