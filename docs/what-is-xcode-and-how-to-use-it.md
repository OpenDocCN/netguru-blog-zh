# Xcode 是什么，怎么用？

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/what-is-xcode-and-how-to-use-it>

 Xcode 是 Apple 的集成开发环境，用于为 macOS、iOS、iPadOS、watchOS 和 tvOS 开发软件。在本文中，我们将回答一些关于这个工具的基本问题。如何使用 Xcode 并提高我们的工作效率？Xcode 的必备工具有哪些？如何学习 Xcode？

## Xcode 是什么？

[Xcode](/web/20221207141209/https://www.netguru.com/services/xcode-development) 是苹果面向苹果所有平台的集成开发环境(IDE)，对所有苹果用户免费。Xcode 提供了为所有 Apple 平台创建应用程序(设计、开发和发布)的所有工具: [iOS、](/web/20221207141209/https://www.netguru.com/services/mobile-development/ios-app-development) iPadOS、tvOS、watchOS 和 macOS。此外，Xcode 支持许多流行编程语言的源代码，包括 Swift、Objective-C、Objective-C++、C、C++、Java、Python 等等。

Xcode 是在苹果应用商店上创建和发布应用的唯一官方工具。

Xcode 的新版本(Xcode 14.0.1)旨在比以前的版本更轻、更快，并允许开发人员使用 Swift 和 SwiftUI 创建多平台应用程序。

![Xcode_IDE_general_point_of_view](img/91b521b4a1a39a3bf502954be76268a9.png)

来源:[苹果开发者](http://web.archive.org/web/20221207141209/https://developer.apple.com/xcode/)

描述:Xcode IDE 一般观点

## 使用 Xcode 的优势

使用 Xcode 作为苹果设备软件开发的高效 IDE 有无数的好处。此外，Xcode 对于代码签名和提交到商店创建的应用程序是必需的。

以下是需要提及的最重要的优势:

*   用户友好的界面
*   出色的代码完成支持
*   高端应用测试
*   内置开发人员工具

*   适用于所有苹果设备的模拟器
*   用于监控软件高级行为的工具
*   用于比较不同版本文件内容的文件合并

*   定制和加速开发过程的扩展
*   与 Xcode 完全集成，用于将项目移动到 CI/CD 平台

## Xcode 怎么用？

Xcode 是为初学者和有经验的开发者设计的。以下是高效使用 Xcode 创建原生应用程序的步骤。

### 怎么安装 Xcode？

Xcode 可以通过两种方式安装在 macOS 上:

![Xcode_App_in_AppStore](img/12ddc49208e4ec8419f0e7b535b5708f.png)*Source:* *Apple Store, Xcode App**Description: Xcode App in AppStore*

*   如果你有苹果开发者账户，可以从苹果网站下载 Xcode。想发布 app 可以免费开开发者账号，还有付费版的苹果开发者账号。

值得一提的是，Xcode 是为 macOS 设计开发的，但是可以在 Windows 上安装运行。但是，我们必须记住，在这种情况下，本机版本的性能可能不太好。因此，强烈建议在原生 Mac 电脑上使用 Xcode，并在为 Windows 构建应用程序时更具选择性。

但是，如果您想在 Windows 上运行它，您可以使用以下替代方法:

*   通过各种云提供商租赁 mac 电脑(例如 [MacStadium](http://web.archive.org/web/20221207141209/https://www.macstadium.com/) ， [Macincloud](http://web.archive.org/web/20221207141209/https://www.macincloud.com/) )
*   创建本地虚拟机，安装 macOS 然后安装 Xcode

### 如何安装 Xcode 的命令行工具？

Xcode 命令行工具是软件开发最需要的工具。软件开发人员在终端(也称为控制台)的命令行上运行它。

在终端中输入命令 xcode-select-install tools 开始安装过程。在这个命令之后，被执行的终端将弹出一个安装 Xcode 命令行工具的提示。

![command_tool_installation](img/d09ac947a46edd2f5f9bb468c194adf7.png)

*描述:命令工具安装*

点按“安装”以开始下载和安装过程，在此过程之后会出现一条确认消息(使用 xcode-select -p 命令来帮助您验证安装状态)。

最后一步是从 Xcode 中选择命令行工具(设置->位置)。

![Settings_screens_section_of_Xcode_IDE](img/aadb850248e9009bd33b8f1525604cd9.png)

*描述:Xcode IDE 的设置屏幕部分，我们可以启用命令行工具*

### 如何使用 Xcode 来提升你的工作效率？

有几种方法可以提高我们在 Xcode 中的工作效率，比如使用扩展或键盘快捷键。

### 1.使用扩展

Xcode 提供了扩展功能，允许开发人员自定义他们的工作空间。这里有一些方法可以提高你的工作效率:

*   Xcode color–这个插件允许开发者在 Xcode 控制台中使用颜色，并帮助他们识别调试语句或错误。
*   这个插件允许开发者直接在 Xcode 中导出代码片段。
*   [swiftPlantUML](http://web.archive.org/web/20221207141209/https://github.com/MarcoEidinger/SwiftPlantUML)–该插件通过命令行界面(CLI)和 Swift 包从 Swift 代码生成 UML 类图。
*   [Import](http://web.archive.org/web/20221207141209/https://github.com/markohlebar/Import)–这个 Xcode 扩展从代码中的任何地方添加导入。

### 2.使用键盘快捷键

#### 常见快捷方式

*   构建项目:⌘ + B(命令+ B)
*   运行项目:⌘ + R (Command + R)
*   测试项目:⌘ + U(命令+ U)
*   停止项目:⌘ +。(Command +。)
*   清洁项目:⌘ + ⇧ + K (Command + Shift + K)
*   清理构建文件夹:⌘ + ⇧ + ⌥ + K (Command + Shift + Option + K)
*   快速打开:⇧ + ⌘ + O (Shift + Command + O)
*   在项目导航器中显示文件:⌘ + ⇧ + J (Command + Shift + J)
*   打开库:⌘ + ⇧ + L (Command + Shift + L)

#### 其他有用的快捷方式

*   打开新标签页:⌘ + T (Command + T)
*   导航项目:⌘ + 1(命令+ 1)
*   导航源代码管理:⌘ + 2(命令+ 2)
*   导航符号:⌘ + 3(命令+ 3)
*   导航查找:⌘ + 4(命令+ 4)
*   导航问题:⌘ + 5(命令+ 5)
*   导航测试:⌘ + 6(命令+ 6)
*   导航调试:⌘ + 7(命令+ 7)
*   导航分组讨论:⌘ + 8(命令+ 8)
*   导航报告:⌘ + 9(命令+ 9)
*   修复范围内的所有错误:⌃+⌥+⌘+f(control+option+command+f)
*   重新缩进代码选择:⌃+ I (Control + I)
*   代码折叠:⌃+⌘+↓(control+command+左箭头键)
*   代码展开:⌃+ ⌘ + ← (Control + Command +右箭头键)

## 如何学习 Xcode？

[苹果官方文档](http://web.archive.org/web/20221207141209/https://developer.apple.com/documentation/xcode)提供了关于 Xcode 最广泛、最详细的观点。此外，还有许多在线资源可以帮助您加深对 Xcode 的理解。以下是其中的一些例子:

## 苹果平台的本地开发

Xcode 由一套工具组成，开发人员使用这套工具为 Apple 平台构建应用程序。Xcode 让您可以管理整个开发工作流程，从创建应用程序到测试、优化和提交到 App Store。

苹果一直在更新 Xcode，让它对开发者来说更加用户友好、快捷、高效和稳定。因此，当[为苹果平台](/web/20221207141209/https://www.netguru.com/services/mobile-development/ios-app-development)开发软件时，Xcode 是原生开发的最佳选择。