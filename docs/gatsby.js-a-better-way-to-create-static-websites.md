# Gatsby . js——创建静态网站的更好方法

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/gatsby.js-a-better-way-to-create-static-websites>

 如果你已经在前端行业工作了几年，你可能还记得使用普通的 HTML、CSS 和 JavaScript(我想还有少量的 jQuery)创建网站是什么样子的。如果你这样做了，你还记得作为前端开发人员的痛苦——处理网页排版的复杂性，努力进行图像优化，自己为各种屏幕分辨率准备合适的图像尺寸。想象一下你在任务上浪费了多少时间。

幸运的是，前端开发的状态已经得到了很大的改善，每年我们都会获得越来越多的工具来帮助我们创建更好、更快、更高性能的网站。其中一个工具是 Gatsby . js——一个现代的[静态站点生成器](/web/20221007082346/https://www.netguru.com/blog/what-are-static-site-generators),基于已知的广受欢迎的 React。

但是静态站点生成器到底是什么——你可能会问。嗯，这是一个工具，让你创建网站的基础上，静态文件，在简单和愉快的开发方式。“静态”意味着您的网站中不会有任何动态加载的内容。一旦你建立并部署了你的网站，它就保持不变，你不能通过 CMS 或 API 来改变它。不过，您可以使用 CMS 来更改内容，这将触发新的构建，并交换已经部署在服务器上的内容。结果，你得到了简单的 HTML/CSS/js 站点，它是轻量级的，非常容易托管。

有了 Gatsby，您可以轻松处理您的项目复杂性，因为您可以使用 React 组件，当谈到前端项目的可伸缩性时，这些组件简直太棒了。此外，没有必要担心诸如排版，复杂的风格，代码的可重用性，图像优化等等！别说了——来吧，我给你看些代码！

### 设置

要开始 Gatsby 之旅，您只需将 gatsby-cli 作为一个全局包安装，并使用它创建一个新项目。

![](img/4d79cb6577a725c39ba87ac405efe745.png)

它将创建一个带有索引页和 404 的简单样板文件。从现在开始，您可以通过简单地添加其他页面来创建自己的页面。js 文件放入“pages”文件夹。你可能会问如何为他们提供一个合适的路线？只需使用 Gatsby 链接组件，并将文件名传递到“to”属性中。

![](img/c4b462e20ac779d218fd15d4864bf465.png)

我们不会太深入，因为我有更令人兴奋的东西给你看。毕竟可以随时查文档，写的真的很好。你可以在这里查看。

## 插件

盖茨比的力量在于插件。这些小代码将节省你在第一段提到的所有重复性任务上的时间。检查盖茨比的插件目录-https://www.gatsbyjs.org/plugins/。基本上什么都有插件！当你完成设置后(只需要几分钟)，你就可以进入更令人兴奋的部分，那就是给你的网站添加适当的插件，这样它就可以利用现代前端工具的真正力量。ESLint、Styled Components、Typography.js 唾手可得。只需将它们安装到您的存储库中，并在 gatsby-config.js 文件中声明它们。

如果你仍然不相信用 Gatsby 开发 site 真的很有趣，看看 Dan Abramov 是怎么说的。

![](img/e2369b00cb851164f0d7d43f0ef22057.png)

### 处理图像最聪明的方法

我想在这里多探讨一点的是管理你的视觉资产。对于开发人员来说，处理多个。jpg 和。具有适当尺寸、大小等的 png。Gatsby 似乎减轻了这种压力，它让您可以只加载一个大文件，这个文件在构建期间会被压缩。它还允许您使用同一图像的多个版本创建 sourceset，以便每个设备只能下载一个适合自己的最佳大小。基本设置应该如下所示:

首先，我们需要设置我们的 gatsby-config.js，以便它能够解析图像文件。得益于此，我们将能够创建 GraphQL 查询。sharp transformer 插件将允许我们在图像上应用复杂的操作。

![configgatsby](img/d0d4ded1cac5582f2b415d06065e18a1.png)

现在，让我们添加一个图像到我们的资产/图像文件夹。之后，让我们进入 index.js 页面，在这里我们可以编写第一个 GraphQL 查询。我们的图像文件名是 winter.jpg，所以它很简单，但是如果这个名字像 background-image-winter-2x.jpg 一样复杂，那么我们的正则表达式就派上用场了，因为 winter 正则表达式只要添加一些特殊的符号就可以解决这个问题。不要忘记超级酷的 imageSharp 功能，它可以让你对你的文件做一些魔术。流体属性允许您创建具有最大高度和宽度的响应图像，并且–您可以通过设置质量(从 0 到 100)轻松压缩文件。

![](img/074fc8c512be53ed2e70c462eaa10819.png)

之后，在我们的 Gatsby 控制台(如果它运行的话)中，我们将能够看到，gatsby-cli 正在为我们的源集压缩和创建多个版本。

![](img/5e22b12aa16532a234b16e3ed428b589.png)

接下来，我们需要将数据属性析构到我们的 IndexPage 组件中——从这个属性中，我们将能够提取转换后的图像。当 GraphQL 查询出现在组件文件中时，它是 Gatsby 自动提供的一个属性。在这种情况下，我们从查询中获取 srcSet，这样我们就可以将它应用到简单的标记中。看看这个:

![](img/e7809851699018b9c87367d0c9cf1e53.png)

我们到了。我们的图像显示在屏幕上，并通过加载最佳尺寸的文件来调整大小。

![](img/fc722b6e64b9b25f5efa6a683b1dc118.png)

更酷的是，我们可以忘记 srcSet 和标记，使用 gatsby-image 组件，它可以利用 sharp image transformer 提供的功能。我们收到的不是简单的 srcSet 字符串，而是 GatsbyImageSharpFluid 对象，它以 base64 编码图像——现在它 100%响应，甚至比 srcSet 更智能。

![](img/2175ec2d1d881310fe4552136d3ee3cd.png)

还有一个更棒的功能——base64 在加载时提供了模糊效果，这给用户带来了即时页面渲染的感觉。别忘了检查使用 sharp transformer 进行图像编码的其他可能性——你会在 [盖茨比-图像文档](http://web.archive.org/web/20221007082346/https://www.gatsbyjs.org/packages/gatsby-image/)中找到。

### 那我能在哪里举办呢？

这一部分可能是最激动人心的。要从所有这些 React 内容中生成一个网站，您只需运行 gatsby build 命令。之后，在你的公共文件夹中，你会发现一个静态页面，你可以在任何主机上托管它——甚至在 s3 存储桶上！但是在静态页面的表面简单性之下，您会发现超级智能的解决方案，如 PWA 功能、积极的预取、base64 编码的响应图像等等！现在你自己去试试吧！

### 但是 2019 年还值得用吗？

当然啦！Gatsby 得到了社区和核心团队的广泛支持(显然甚至得到了 React 的创作者的支持)。它肯定会统治静态网站的市场，因为没有其他解决方案能以如此少的投入提供如此多的东西。看看我们只用了几步就取得了什么成就吧！我们已经创建了一个具有路由和图像处理功能的全功能静态网站。我们的网站提供开箱即用的 PWA 功能，并在绩效审计中获得最高分。如果我们选择在经典的、普通的 HTML/CSS/js 前端堆栈中做同样的事情，事情会复杂得多。

* * *

照片由 [马库斯·斯皮斯克](http://web.archive.org/web/20221007082346/https://unsplash.com/photos/8OyKWQgBsKQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [下](http://web.archive.org/web/20221007082346/https://unsplash.com/search/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)