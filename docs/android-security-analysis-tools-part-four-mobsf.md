# Android 安全分析工具，第四部分- MobSF

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/android-security-analysis-tools-part-four-mobsf>

 本文是系列文章的一部分:

[Android 安全分析工具，第一部分- JAADAS](/web/20220924170138/https://www.netguru.com/blog/android-security-an-overview-of-static-analysis-tools-part-one)

[Android 安全分析工具，第二部分- DIVA 应用和 Android bugs](/web/20220924170138/https://www.netguru.com/blog/android-security-analysis-tools-part-two-androbugs)

[Android 安全分析工具，第三部分- Drozer 和 QARK](/web/20220924170138/https://www.netguru.com/blog/android-security-analysis-tools-part-three-drozer-and-qark)

Android 安全分析工具，第四部分- MobSF

我们呈现致力于 Android 应用安全分析工具的系列博文的最后一部分。本系列中讨论的工具是由 OWASP 移动测试指南(MSTG)建议的。在前面的部分中，我们讨论了[贾达斯](/web/20220924170138/https://www.netguru.com/blog/android-security-an-overview-of-static-analysis-tools-part-one)、[和](/web/20220924170138/https://www.netguru.com/codestories/android-security-analysis-tools-part-two-androbugs)、[德罗泽和 QARK](/web/20220924170138/https://www.netguru.com/blog/android-security-analysis-tools-part-three-drozer-and-qark) 。这一章将集中讨论 MobSF，这也是 MSTG 建议的。概述的主要目标是找到最好的工具，它也最容易与现有的 CI/CD 堆栈集成。

## 不仅仅是一个控制台

MobSF，也称为[移动安全框架](http://web.archive.org/web/20220924170138/https://github.com/MobSF/Mobile-Security-Framework-MobSF)，是 OWASP MSTG 建议的另一个工具，用于移动应用程序的静态安全分析。它旨在对最常见的移动平台(Android、iOS 和 Windows)进行静态和动态的安全性分析和测试。与前面讨论的工具不同，MobSF 有一个 web 服务形式的图形用户界面。web 服务由一个显示分析结果的仪表板、自己的文档站点、一个集成的仿真器/symulator 和一个允许用户自动触发分析的 API 组成。正如我提到的，MobSF 支持不同的移动平台，因此用户能够将 APK、IPA 或 APPX 文件发送到仪表板来触发分析过程。该过程将在用户发送应用程序时开始。该工具将对其进行反编译，并对反编译后的源代码进行分析。分析也可以在压缩的源代码上执行。

MobSF 是一个主要用 Python 编写的框架，支持 JavaScript、Smali 代码和 shell 命令。最新的版本是 1.0Beta，在 GPL 3.0 许可下发布。

该框架是一个非常强大的工具，尤其是它能够测试多个移动平台。该特征是该工具区别于之前概述的工具的另一个方面。然而，这个框架的最大优势当然是它可以找到的分析类型和漏洞的列表。以下是一些最有趣的功能:

*   基本申请信息扫描；
*   应用组件检测和配置检测(活动、服务、内容提供商等。对于安卓)；
*   清单文件分析；
*   源代码反编译；
*   证书和签名分析；
*   权限分析和分类；
*   本地库分析(例如，检测 C 库的错误配置的 ASLR)；
*   SDK 误用检测；
*   代码问题(例如，包含敏感信息的日志、未删除的 tmp 文件、原始 SQL 查询的执行等。);
*   恶意软件检测；
*   外部 API 通信检测；
*   资源分析；
*   检测代码或资源中的敏感信息；
*   等等。

此外，还可以在完成静态分析后执行动态分析。这是可能的，因为仿真器集成在设备中。模拟器允许您创建不同的环境，测试应用程序，并检查分析期间发布的日志。

## 综合

MobSF 将其源代码发布在 GitHub 上的这个[存储库中。此外，该框架以随时可用的形式作为 Docker 容器发布。这个容器发布在名为](http://web.archive.org/web/20220924170138/https://github.com/MobSF/Mobile-Security-Framework-MobSF) [DockerHub](http://web.archive.org/web/20220924170138/https://hub.docker.com/r/opensecurity/mobile-security-framework-mobsf/) 的 Docker 存储库中。由于这种方法，安装 MobSF 服务非常容易。要安装和运行这个工具，你只需要安装 Docker，然后你就可以运行这两个命令:

```
docker pull opensecurity/mobile-security-framework-mobsf
```

```
docker run -it -p 8000:8000 opensecurity/mobile-security-framework-mobsf:latest
```

第一个命令将下载容器 DockerHub。第二个将使用端口 8000 上的容器运行 Docker。得益于此，您可以轻松地将该工具集成到您的工具库中。此外，由于 API 是 MobSF 框架的一个集成部分，集成变得更加容易。由于这个集成的 API，您可以使用标准的 HTTP 请求轻松发送应用程序、触发分析和收集结果。这使得集成非常容易和灵活。不幸的是，该框架不包含任何类型的访问管理，所以仪表板很容易访问。此外，该工具不支持 HTTPs 加密。这意味着，为了确保分析结果的安全，该工具应该安装在 CI 环境内部的本地网络中，或者安装在任何与互联网接入无关的地方。当然，该工具的源代码是可用的，因此可以实现额外的访问管理功能。但是，我们需要记住的是，在正式版本中没有这样的功能。

## 连接

如前所述，GUI 是该工具与众不同的地方之一。它增加了工具的可用性，因为通过 web 浏览器管理它比仅仅使用控制台要容易得多。这使得该工具更加用户友好，因此执行分析和查看结果所需的时间可以显著减少。

![](img/ccb71c3ef22bfd5539363a284ea6b4cf.png)

如您所见，该界面是一个简单的基于 Python 的网页。所有分析都是在应用程序上传阶段进行的。然后框架会生成一个结果网页，你可以在上面的截图中看到。它包含有关被分析应用程序的所有信息，这些信息被分成不同的类别。这些类别基于应用分析的领域。如您所见，主菜单还包含下载报告和开始动态分析按钮。第一个将生成一个 PDF 文件形式的报告。它将包含网页上显示的所有结果。*启动动态分析*将启动包含仿真器的网页。

![Zrzut ekranu 2018-12-10 o 08.33.58](img/0247a06705734f467b613409c64a6b96.png)

不幸的是，模拟器仍然有一些问题，很难运行。我希望这个特性很快得到修复——未来看起来很有希望。

值得一提的是，主仪表板包含到 API 文档和历史网页的链接。“历史记录”页面包含最近扫描及其结果的列表。最近的扫描可以很容易地探索由于包括搜索引擎。

## 赞成的意见

*   易于安装——基于 Docker 容器；
*   使用集成的 API，易于与现有 CI/CD 堆栈集成；
*   丰富的图形界面；
*   应用程序反编译的复杂和多层分析；
*   准备好在多种移动平台上分析安全性——Android、iOS，甚至 Windows
*   可以使用内置仿真器/模拟器直接对分析的应用程序执行手动测试。
*   由于框架存储的历史结果，可以比较分析结果；
*   可以生成和下载 PDF 格式的报告；
*   可以使用集成仿真器在设备上仿真系统事件、管理系统进程或分析文件；
*   是不断发展的；
*   开源
*   基于 apktool 和 Dex2jar 等经过良好测试的工具；
*   可以在 JSON 中生成结果。

## 骗局

*   缺少访问管理功能；
*   该框架仍处于测试阶段；
*   很难运行 Android 模拟器。

## 概观

当然，MobSF 是一个非常强大和复杂的工具。此外，作为唯一一个讨论的工具，MobSF 有一个广泛的图形界面，这使得它更加用户友好。由于这些方面，该工具非常适合快速分析移动应用程序或回归测试。此外，由于易于安装和集成的 Web API，该工具还可以集成到您当前的 CI/CD 堆栈中。在我看来，由于它的灵活性，它是一个非常好的工具，可以完美地适应许多现有的过程。我希望该工具将继续得到开发和改进。

下一步是什么？

## 在对 OWASP 建议的几个工具进行审查之后，我们决定选择两个工具:QARK 和 MobSF。在我们看来，就功能而言，它们是最好的。它们也符合我们的第二个目标——它们很容易与现有的堆栈集成。鉴于这些方面，我们将在团队中对这两个工具进行测试实现。这些测试将让我们收集更多的信息，了解哪一个更适合我们的需求。测试后，我们将选择一个工具，并发布一个关于我们结论的简短案例研究。

继续阅读:

* * *

[Android 安全分析工具，第一部分——JAADAS](/web/20220924170138/https://www.netguru.com/blog/android-security-an-overview-of-static-analysis-tools-part-one)

[【Android 安全分析工具，下篇——DIVA app 和 Android bugs](/web/20220924170138/https://www.netguru.com/blog/android-security-analysis-tools-part-two-androbugs)

[【Android 安全分析工具，第三部分——Drozer 和 QARK](/web/20220924170138/https://www.netguru.com/blog/android-security-analysis-tools-part-three-drozer-and-qark)

照片由[昆廷·凯梅尔](http://web.archive.org/web/20220924170138/https://unsplash.com/photos/ZXYK4LljnT0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](http://web.archive.org/web/20220924170138/https://unsplash.com/search/photos/hubble-telescope?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)T5 拍摄

Photo by [Quentin Kemmel](http://web.archive.org/web/20220924170138/https://unsplash.com/photos/ZXYK4LljnT0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](http://web.archive.org/web/20220924170138/https://unsplash.com/search/photos/hubble-telescope?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)