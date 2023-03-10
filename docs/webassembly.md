# web 程序集

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/webassembly>

 什么是 WebAssembly

“WebAssembly 是一种可以在现代 web 浏览器中运行的新型代码，它是一种低级的类似汇编的语言，具有紧凑的二进制格式，以接近本机的性能运行，并为 C/C++等语言提供编译目标，以便它们可以在 web 上运行。它还被设计成与 JavaScript 并行运行，允许两者协同工作。”- Mozilla 开发者网络。

WebAssembly ( **WASM** )基本上假设所有代码都将在 JavaScript 沙盒运行时中以与 JavaScript 相同的方式执行。这意味着 WebAssembly 应用程序和 JavaScript 代码之间的内存是共享的，但是是有限的。对 Web APIs 的访问也受到限制，必须通过 JavaScript。

WASM 通常比 Javascript 快，因为它已经是二进制格式，JavaScript 运行时无需解释语言就能理解。所以这意味着解码 WebAssembly 比解析 JavaScript 花费的时间要少。编译和优化也需要更少的时间，因为**更接近机器码**，不需要重新优化。根据功能和复杂性，WebAssembly 的运行速度从 10%到 800%不等。

![Simplified client-side architecture scheme.](img/abdb67b839f7e25ae2fce0fdc2fc03f6.png)

*简化客户端架构方案*

WebAssembly 主要关注 CPU 密集型任务，可成功用于:

*   比赛

*   图像、视频、音频或 CAD 流和编辑

*   虚拟现实和增强现实

*   仿真或模拟平台

*   加密

*   人工智能

*   P2P 应用

*   VPN 和远程桌面

*   物联网

*   机器学习

## **构建到 web 程序集**

非常重要的是，WebAssembly 在所有主流浏览器中都得到支持:Mozilla Firefox、Google Chrome、Safari 和 Edge，这是实现其假设所必需的，比如可移植性。主要目标是在几乎任何地方运行 WASM，从浏览器到服务器端以及嵌入式系统。WebAssembly 的目标与 Java 试图通过小程序实现的目标相同。

因为 WASM 旨在成为 C、C++、Rust 等低级源语言的有效编译目标，所以使用这些语言之一来创建 WebAssembly 模块是一个自然的选择。为什么能成为一个好的选择？因为在这些语言中我们是手动管理内存的，而当前版本的 WebAssembly 根本不支持垃圾回收。

然而，这并不意味着我们不能使用其他使用该功能的编程语言，如 Python、Go 或 C#。这将要求我们使用额外的工具或解释器来实现特定语言的功能。

WebAssembly 支持的语言 ***(需要第三方工具或翻译):***

*   **T1。网**

*   ***汇编脚本***

*   C

*   ***c#***

***   C++

    *   ***D***

    *   向外

    *   ***走***

    *   ***Java***

    *   ***伊德里斯***

    *   ***科特林/本土***

    *   左上臂

    *   ***Perl***

    *   ***PHP***

    *   ***诗歌***

    *   ***Python***

    *   ***序言***

    *   ***红宝石***

    *   锈

    *   ***方案***

    *   ***哇***

    *   ***沃尔特***

    *   ***Wam***** 

 **WebAssembly 由两种表示相同结构的格式组成，但方式不同:

**实施例**

## 举个例子，假设我们在 Rust 中创建了一个简单的 square 函数，我们将在 JavaScript 中使用它在容器中显示结果。我们假设 WASM 编译是成功的，并且我们收到了一个准备好的文件，可以作为模块导入。

我们的函数看起来像这样:

此时，我们要做的就是创建一个 JavaScript 文件，并使用 **fetch()** 和**WebAssembly . instantiate()**来获取、编译和实例化 web assembly 代码。这样，我们就可以访问我们的**广场**功能。代码如下所示:

Our function looks like this:

**现实世界应用**

![Example function](img/635a82c308138030e9284f453d8e1dbb.png)and after compilation, we get this:

如何完美地使用 WebAssembly 的最好例子是 **AutoCAD** 。在几次尝试其旗舰产品的网络应用程序后， **Autodesk** 决定使用 WASM。现在是紧挨着 **React.js** (UI 层)的应用层之一。之前，他们尝试了 Adobe Flash 和 HTML5 与 JavaScript。有趣的事实是，在将代码库编译为 WebAssembly 之前，他们必须删除 Windows API 的所有依赖项。

![Code after compilation](img/6e7397e34d37af0ec8df130604b33e4c.png)Now we can prepare the basic HTML structure in our index.html![HTML code](img/4277e90ca6cb8af737183ba41301fdc7.png)

WebAssembly 提供的可能性的另一个例子是允许你开发游戏的 **Construct 3** 和设计者的工具 **Figma** 。还值得一提的是**PSP PDF kit**-一款生成 PDF 文档的应用程序和 **ACTIV Financial** -实时和多资产金融市场数据和解决方案提供商。

![JS code](img/f94c01bc60c09817e24beba308f934f7.png)

**含义**

## 当前版本的 WebAssembly 中缺少的一个部分是，它们不能直接访问像 DOM、CSSOM、WebGL、IndexedDB、Web Audio API 等 Web APIs。所以如果你想在你的 WebAssembly 模块中访问一些特定于平台的 API，你需要用 JavaScript 调用它。当然，这样的操作对于那些 JavaScript 调用来说是一种代价。

与 JavaScript 一样，WebAssembly 运行在沙盒执行环境中，这意味着它将强制执行浏览器的同源权限策略，并且也不能访问文件系统。

一个相当大的不便是浏览器调试器支持还没有真正实现。

![Simplified AutoCAD application layers](img/7111faeefefa43c8be4084fa4585751f.png)*Simplified AutoCAD application layers.*

**局限性** ***(缺失部分)*** **:**

## 多线程操作

单指令多数据(SIMD)

碎片帐集

ES 模块集成

异常处理

JavaScript 和 WASM 之间的快速交互和数据交换

*   Wasm64(支持 wasm32 模式，线性内存大小高达 4 GiB)

*   多值回报

*   BigInt 转换

*   **前端开发者视角**

*   那么，WebAssembly 是不是属于[前端开发者](/web/20220929114645/https://www.netguru.com/services/front-end-development)的技术？在我看来，并不是。当然，我们将使用它，但是基于 API 通信。WASM 是一种技术，它肯定会给后端开发人员更多空间来提高性能，[开发可扩展的 web 应用程序](/web/20220929114645/https://www.netguru.com/services/web-development)。WebAssembly 和其他两种 web 技术的潜在用途看起来非常有趣，对我来说，它们是完美的互补。

*   第一个组合是 WASM 与 [**渐进式网络应用**](/web/20220929114645/https://www.netguru.com/services/progressive-web-app-development) (PWA)。这是一个非常明显的组合，考虑到 PWA 为常规网站和 web 应用程序(例如离线工作)提供类似于本机应用程序的用户功能。

*   第二个是拥有 **Web 组件**的 WASM。这两种技术的结合为开发人员提供了创建高级 HTML 组件的机会。举个简单的例子，我们将能够开发一个内置编解码器的视频播放器组件。当然，这个解决方案提供了各种各样的可能性，像任何文件类型查看器甚至编辑器。

*   **总结**

*   WebAssembly 最重要的优势之一是，它允许开发人员创建在质量和性能上与本机桌面应用程序没有区别的 web 应用程序。与传统应用程序相比，开发和维护软件本身的成本会更低。这是因为应用程序的分发仅限于浏览器，同时覆盖所有可能的操作系统。此外，将桌面应用程序转换为 web 应用程序的成本应该不会很高，因为我们可以使用现有的代码，并将其重新编译为 WebAssembly，最完美的例子是 Autodesk。

## 但是，应该记住，WebAssembly 是一项相对较新的技术，它的局限性肯定会使我们很难决定它的潜在用途。

**建议**

WebAssembly 的开发和后续迭代应该**明确**被**观察**。然而，我认为在目前的项目中使用它的可能性可能是一个普通的**矫枉过正**。根据我的评估，WebAssembly 的使用只有在严格定义的情况下才有意义，主要是**大型**规模**应用**。

**附录**

## **Webassembly 工作室**

是 Mozilla 开发的一个在线 IDE，可以用来编译 C/C++和 Rust 代码到 WebAssembly 中。

[https://webassembly.studio/](http://web.archive.org/web/20220929114645/https://webassembly.studio/)

## **Emscripten**

是 JavaScript 和 WebAssembly 编译器的开源 LLVM。让开发人员能够编译用/ C ++编写的项目，还内置了 OpenGL 到 WebGL 的转换，并允许我们直接使用熟悉的 API，如 SDL 或 HTML5。

## **asm.js**

### 是 JavaScript 的一个非常优化的低级子集，设计用于在浏览器中运行 C/C++代码，这些代码以前由编译器翻译，如 **Emscripten** 。这个解决方案正慢慢被 WASM 取代。然而，在 WebAssembly 的当前阶段，它在某些功能上受到限制，例如，在浏览器中调试代码，asm.js 可以提供帮助。

布拉索

是一个基于 C#、Razor、Mono 和 HTML 的. NET web 框架，通过 WebAssembly 在 web 浏览器中运行。开发人员可以用 C#或 VisualBasic.NET 编写代码，然后编译成普通代码。NET 程序集。代码将在使用基于 WebAssembly 的 web 浏览器中运行。NET 运行时。

### **Emscripten**

Is an Open Source LLVM to JavaScript and WebAssembly compiler. Gives the developer the ability to compile projects written in / C ++, also has a built-in conversion OpenGL into WebGL, and allows us to use familiar APIs like SDL, or HTML5 directly.

### *简化客户端架构方案*

**参考文献**

### [R&D-web assembly-Articles](http://web.archive.org/web/20220929114645/https://docs.google.com/spreadsheets/d/1gl6PxELXtTQubJsIQh4VFwFMEFn9IDYgOcC3kOf2jLE/edit#gid=0)

照片由 [克里斯·利维拉尼](http://web.archive.org/web/20220929114645/https://unsplash.com/photos/HUJDz6CJEaM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [下](http://web.archive.org/web/20220929114645/https://unsplash.com/search/photos/speed?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

![Simplified client-side architecture scheme](img/8e719f7f53a815dbd8e7868cc4e3a3ed.png)

*Simplified client-side architecture scheme*

## **References**

[R&D - WebAssembly - Articles](http://web.archive.org/web/20220929114645/https://docs.google.com/spreadsheets/d/1gl6PxELXtTQubJsIQh4VFwFMEFn9IDYgOcC3kOf2jLE/edit#gid=0)

* * *

Photo by [Chris Liverani](http://web.archive.org/web/20220929114645/https://unsplash.com/photos/HUJDz6CJEaM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](http://web.archive.org/web/20220929114645/https://unsplash.com/search/photos/speed?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)**