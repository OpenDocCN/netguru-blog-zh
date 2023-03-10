# 开始新的合成多平台项目

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/starting-compose-multiplatform-project>

 Compose Multiplatform (CM)是谷歌与 Android Studio 和 IntelliJ 背后的公司合作的结果。这是一个 UI 框架，通过使用 Kotlin 简化并加速了桌面应用程序的开发。CM 使用与 Jetpack Compose 相同的部分技术，但将其扩展到跨平台工作。

Compose 多平台框架将 Android 开发人员熟知的声明性 UI 模型带到了其他平台，包括:

*   桌面应用程序(Linux、macOS 和 Windows)
*   webapps
*   iOS 应用程序(目前正在开发中)

本技术指南旨在解释如何创建自己的 Compose 多平台项目、添加模块和执行基本设置。

## 撰写多平台应用程序模板

要开始一个[新的构建多平台项目](/web/20221006015908/https://www.netguru.com/blog/compose-multiplatform-custom-charts)，你首先需要安装 [IntelliJ IDEA](http://web.archive.org/web/20221006015908/https://www.jetbrains.com/idea/)

选择`New Project`后，继续选择`Kotlin`。然后，从可用的模板中选择`Compose Multiplatform Application` …。

请注意，您的 JDK 必须至少是 JDK 11。此外，为了能够使用本地分发包，您必须使用 JDK 15 或更高版本。

为新项目选择名称和位置后，单击`Next`。

*新项目创建者*

![new_project_creator_compose_multiplatform](img/fa8a64437e10e7a2c95421f07551e8f8.png)

在 Android 上使用时，会提示你提供 Android SDK 的路径。您可以使用与您的 Android Studio 安装捆绑在一起的那个，它应该位于`/Users/<your name>/Library/Android/sdk.`

*模块设置*

在这个阶段，您还可以单击您的模块和目标。您可以根据需要修改它们。

![modul_setup](img/4345792e422f36842af30608dfb0f329.png)

*Module setup*

At this stage, you can also click on your modules and targets. You can modify them to your needs.

*可用模块列表*

![modules_targets](img/c278d2fd15e92f3503d542fca02aaa69.png)You can also include additional modules, such as Web or iOS, by selecting `Project` in the menu and clicking (or right-clicking) the `+` button.

添加完`browser`模块后，如果您想在`browser`模块中引用它，您可以继续向`common`模块添加一个`js`目标。

![list_modules](img/27a23bf578aadf94a263bb91a8f1913d.png)

您也可以通过右击并选择`Remove …:.`来删除任何不想要的`modules`和`targets`

如果您已经准备好配置，单击 Finish 跳转到您的新项目。这应该是模板配置的结果:

模块及其用途

![browser_module](img/9787af06a65f0ec59b769cda00114747.png)

现在，每个平台的模块目录— `Android`、`browser`和`desktop` —已经生成。它们可能是您认为的应用程序的入口点。例如，`Android`模块可以被认为是 Android 多模块项目中的传统`app`模块。您已经将主要活动放在那里，并且可以从里面设置来自`common`模块的内容。

这些特定于平台的模块都引用了`common`模块，您可以在其中放置公共代码，如组合多平台组件、域和数据代码。

在公共模块中生成的所有目录中，您应该感兴趣的主要目录是`commonMain`,您将在那里放置大多数多平台代码。我将在本指南中进一步解释其他目录。

Web 合成有何不同？

![template_configuration](img/549b4795c358606b867380948fabeed7.png)*Project structure straight out of configuration phase*

尽管 Compose for Web 的工作方式与经典的 Jetpack Compose 类似(即视图由可组合函数组成，当参数值发生变化时，这些函数会重新组合)，但它具有创建布局的独特元素。

## 截至发稿时，还不能使用 Compose 的`Column()`、`Row()`和`Box()`。相反，Compose for Web 中的视图需要由 DOM 元素组成，这些元素可以使用像`Div()`、`Span()`和`Button()`这样的可组合函数来添加，这不同于传统的 Compose 按钮()。要对这些元素进行主题化，您需要使用样式生成器在 Compose 中应用 CSS。

由于这些原因，我将跳过 web 实现，并在本文中进一步讨论。

添加平台特定的可组合组件

## 假设在`commonMain`目录中的`common`模块中，你想要一些下拉菜单的通用代码。问题是(在撰写本指南时)这两个平台没有可组合的公共下拉菜单。正因为如此，我们需要针对每个平台的特定代码。你是怎么做到的？在`commonMain`中，你可以创建一个`CustomDropdown.kt`文件。在里面，使用`expect`关键字:

在`desktopMain`中，您可以创建`DesktopCustomDropdown.kt.`

这个`DropdownMenu`可组合的看起来和 Android 的一模一样。请注意，这可能具有欺骗性。它们是独立的实现，并且是特定于平台的。他们不一样。

```
Div({ style { padding(25.px) } }) {
    Button(attrs = {
        onClick { console.log(“Clicked”) }
    }) {
        Text(“Click me”)
    }
} 
```

在创建了这三个文件之后，您现在可以在`commonMain`目录中使用一个公共的可组合的`CustomDropdownMenu()`。

### 在`androidMain`子目录中，您还需要始终有一个`AndroidManifest.xml`文件，其中包含包的名称，如下所示:

带有`Test`后缀的目录当然是为了测试相应目录中的代码。例如，您应该对`androidTest.`中的`androidMain`代码进行测试

手动添加新目标(使用 web 示例)

```
@Composable
expect fun  CustomDropdownMenu(
    expanded: Boolean,
    onDismissRequest: () -> Unit,
) 
```

如果在 IntelliJ IDEA 中设置项目后需要添加 web 模块，可以按照下列步骤操作:

```
@Composable
actual fun  CustomDropdownMenu(
    expanded: Boolean,
    onDismissRequest: () -> Unit,
) {
    DropdownMenu(
        expanded = expanded,
        onDismissRequest = onDismissRequest,
        focusable = false // Example of a parameter available only on desktop implementation
    ) {
        // desktop dropdown content
    }
} 
```

在 IntelliJ IDEA 中，选择`File`>`New`>`Module`。

选择`Compose Multiplatform` 发电机，选择 `Web` 为平台，并填写其他字段。

```
<?xml version=“1.0” encoding=“utf-8"?>
<manifest xmlns:android=“http://schemas.android.com/apk/res/android”
package=“com.netguru.common” /> 
```

为多平台项目中的依赖关系保留一个真实的来源与常规的多模块 Android 应用程序没有什么不同——如果你愿意，你可以选择使用 buildSrc 的 [Kotlin 对象，或者](http://web.archive.org/web/20221006015908/https://proandroiddev.com/better-dependencies-management-using-buildsrc-kotlin-dsl-eda31cdb81bf) [Gradle 版本目录](http://web.archive.org/web/20221006015908/https://proandroiddev.com/gradle-version-catalogs-for-an-awesome-dependency-management-f2ba700ff894)。

### 当决定在多平台项目中使用新的库时，您需要记住这一点。

虽然这个组合多平台模板目前只在 IntelliJ IDEA 中可用，但是一旦您创建了所需的初始配置，您就可以返回 Android Studio 来开发您的应用程序。不同的目标，如桌面或网络，可以通过适当的`gradlew`命令运行。

*   随着 Compose Multiplatform 的发展，我将非常乐意分享即将出现的新方法。更多类似于本指南的技术说明，您可以在 Netguru 查看我们的资源库。如果您正在寻找使用 [Kotlin 或编写多平台](/web/20221006015908/https://www.netguru.com/services/kotlin-multiplatform)的帮手，请联系我们，告诉我们更多关于您的项目的信息。
*   Select the `Compose Multiplatform` generator, pick `Web` for the platform, and fill out the other fields.

![compose_multiplatform_generator](img/c219d643ed3b20d65818c32cbc461c6e.png)

Holding a single source of truth for the dependencies in a multiplatform project is no different from a regular multimodule Android application - you can opt in to use [Kotlin objects from buildSrc](http://web.archive.org/web/20221006015908/https://proandroiddev.com/better-dependencies-management-using-buildsrc-kotlin-dsl-eda31cdb81bf), or [Gradle version catalogues](http://web.archive.org/web/20221006015908/https://proandroiddev.com/gradle-version-catalogs-for-an-awesome-dependency-management-f2ba700ff894) if you prefer.

You need to keep this in mind when deciding to use a new library in your multiplatform project.

While this Compose Multiplatform template is at this time available only in IntelliJ IDEA, once you have created the desired initial configuration you can return to Android Studio to develop your application. Different targets, such as Desktop or Web, can be run via appropriate `gradlew` commands.

As Compose Multiplatform evolves, I’ll be more than happy to share new approaches that will surface. For more technical notes similar to this guide, you may check out our library of resources at Netguru. If you’re looking for an extra hand with [Kotlin or Compose Multiplatform](/web/20221006015908/https://www.netguru.com/services/kotlin-multiplatform), reach out to us, and tell us more about your project.