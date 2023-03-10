# 科特林多平台指南。通用模块版本控制

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/kotlin-multiplatform-guidelines.-common-modules-versioning>

 这篇关于 Kotlin 多平台指南的评论将集中在项目架构和设计方面。

## 项目模块化的动机

Kotlin Multiplatform 使得维护一个包含不同目标平台共享代码的项目成为可能，这些平台包括*后端*、 *web 前端*、 *Android* 和 *iOS* 应用等等。这就是为什么这种多平台项目需要高度模块化的原因:

1.  为了达到令人满意的代码可重用性

2.  为一个大而复杂的项目提供一个清晰的架构

在不同的目标之间共享代码有很多好处。在通用后端-前端代码的情况下，好处是用于数据通信的数据模型的一致性。对于 Android、iOS 和 web 前端，所有这些目标都可以共享一个*业务逻辑*层，负责诸如*用户输入检测和处理*、*网络通信*、*数据处理、数据持久化*之类的事情。UI 层应该为每个平台单独实现。

还可能需要将一些额外的多平台代码提取到一个模块中，该模块包含其他不同项目模块所需的实用函数和通用类型模型。它们可以被提取并保存在项目之外，并作为外部多平台依赖项进行链接。

## 模块版本化的需求

为了使各种平台相关模块的同时开发成为可能，需要引入公共(共享)模块的版本控制。

![Screenshot 2019-02-15 at 19.01.40](img/87e3c0e1d63ca2b775760167a53a3a21.png)

让我们考虑下面这个包含 3 个模块的项目的例子: *A* 、 *B* 、 *C* ，其中 A 由 *B* 和 *C* 平台相关模块使用的共享业务逻辑代码组成。

*B* 和 *C* 模块正在内部使用 *A* 模块依赖关系。来自 *A* 模块的共享代码被相应地编译到目标平台(例如 Android 的 jar 文件、iOS 的框架或静态库)并链接到最终产品。每当对由 *A* 模块公开并推送到主分支的 API 进行一些更改时，都需要立即修改 *B* 和 *C* 模块，以便允许构建整个项目。因此，如果 *B* 和 *C* 模块紧密耦合

![Screenshot 2019-02-15 at 19.01.46](img/037bdcd3b753a9bd8f4b0867eb70dc2f.png)the latest *A* module state the project is impossible to maintain as a whole. To make the development more flexible the changes made to the *A* module should be released as the versioned artifacts without a need to adopt all the dependent modules immediately.

正在运行的模块版本控制

发布到 Maven 本地存储库

## **kot Lin-多平台** Gradle 插件与标准 Gradle **maven-publish** 插件配合良好:

## 上面的配置足以在本地提供无痛苦的版本控制和模块工件的发布。模块工件发布在本地 maven 存储库目录下(~/.m2)。

将工件发布到 maven 本地存储库对于新模块版本的开发和测试来说是完美的。

```
apply plugin: 'maven-publish'

// Publication metadata
version '0.0.1'
group 'com.netguru.papersoccer'
```

发布到 Bintray 存储库

当一个模块的新版本准备好发布的时候，就该把它上传到一个外部存储库了。这是允许在项目的其他层中集成新的模块版本，并使项目在 CI 上成功构建所必需的。

## 在与 Kotlin Everywhere 工作组下的项目合作时，我们使用 Bintray 存储库来上传准备好的工件。下面的 **publish-bintray.gradle** 脚本应用于每个项目模块。

使用**调用上传。/gradlew bintrayUpload** 命令。它负责构建项目，生成正确的工件，最后将其上传到 Bintray maven 存储库。

为了给模块公开的 API 提供文档，可以使用 KDoc[https://kotlinlang.org/docs/reference/kotlin-doc.html](http://web.archive.org/web/20220929101222/https://kotlinlang.org/docs/reference/kotlin-doc.html)。

多平台库工件架构

Kotlin-multiplatform 插件允许为以下一组平台构建共享库:

## Java 虚拟机-标准。jar 文件

Android VM -目前(2019 年 1 月)存在生成问题。使用 kotlin-multiplatform 插件的 aar Android 库和一些实现它所需的工具(更多细节可在下面的链接中获得:[https://youtrack.jetbrains.com/issue/KT-27535](http://web.archive.org/web/20220929101222/https://youtrack.jetbrains.com/issue/KT-27535)

1.  Kotlin/Native (iOS、x86、STM32 目标)。klib 包是为 iOS 等原生平台生成的。Klib 本机库工件可以链接到其他 kotlin 多平台和平台本机模块。更多信息请点击这里:[https://kotlinlang . org/docs/tutorials/native/working-with-klib . html](http://web.archive.org/web/20220929101222/https://kotlinlang.org/docs/tutorials/native/working-with-klib.html)

2.  kot Lin-多平台插件与 maven-publish 插件无缝集成。它为模块级构建脚本中定义的每个目标自动生成适当的工件。

3.  Netguru Kotlin 多平台项目示例

[https://github.com/netguru/KotlinMultiplatformStorage](http://web.archive.org/web/20220929101222/https://github.com/netguru/KotlinMultiplatformStorage)

## Netguru Kotlin Multiplatform project examples

1.  [https://github.com/netguru/KotlinMultiplatformStorage](http://web.archive.org/web/20220929101222/https://github.com/netguru/KotlinMultiplatformStorage)