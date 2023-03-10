# 正确的成分——充分利用 AWS、Lambda 和无服务器(更新)

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/aws-lambda-node-js>

 你如何避免在你的网络应用程序上有多个沉重的功能或服务的头痛？

你可以添加更多的资源或重构，或者堆积外部服务——但是当用户回来对新功能有更多的要求时，这并没有帮助。

我们看看如何将 Node.js 与 Lambda AWS 和 Serverless 结合起来解决这些问题。 

假设您的开发人员正在为一个处理图像的项目添加新功能，我们将使用一个照片共享平台作为例子。

所需的功能将存储图像，并分配给他们一个网址-很简单。

但是当用户请求添加裁剪和大小调整功能时会发生什么呢？好吧。

你在网上找到一个合适的解决方案，并将其链接到你的项目，但用户又回来了，这次他们希望你实现一个图像识别功能，将结果存储在某个地方。

事情开始变得复杂，尤其是价格问题。

这就是 AWS 发挥作用的地方:

![AWSInfo-1](img/5be45f225709fea28b29dee880644656.png)

**来源:** [AWS 亚马逊](http://web.archive.org/web/20221209121003/https://aws.amazon.com/enterprise/executive-insights/content/generating-business-value-with-aws-serverless/)

您可以连接所有这些服务，并记住正确的逻辑顺序和异常。显然，该服务必须是异步的，因为只有在成功上传后才会将图像发送到裁剪服务，然后再发送到识别服务。

现在我们看到了问题:许多易受攻击的依赖项和“沉重”的特性。

在如此复杂的环境中开发任何新的东西都需要花费大量的时间，而且为如此多的外部服务付费是非常昂贵的。

答案可以从这些强大的成分中找到:

**node . js+Lambda(AWS)+server less =一个轻量级项目**

![lightweight-project.jpg](img/dc1dbe706988dfa13488546861fd2e6f.png)

以上工具和技术对你来说是新的吗？别担心**、** [下面是我们关于如何设置它们的深入指导。](/web/20221209121003/https://www.netguru.com/blog/3-aws-tricks-for-your-business-lambda-serverless-and-cloud-functions)

为什么要用 Node.js？

### **Node.js** 是一个开源的应用运行时环境，允许你用 Javascript 编写服务器端和网络应用。

它提供了一个强大的运行时环境，允许在服务器端执行 JavaScript 非常适合构建快速和可伸缩的 web 应用程序。

由于一个充满活力的社区确保该技术紧跟最新趋势，它获得了巨大的人气。

像网飞、沃尔玛、优步、Trello、PayPal、LinkedIn、Medium 甚至 NASA 这样的公司都利用 Node.js 作为他们 web 应用程序的后端。

换句话说，这是一个构建真正快速和可扩展的服务器应用的平台。

我们整理了一份[在生产中使用 Node.js 的前 10 家公司的列表](/web/20221209121003/https://www.netguru.com/blog/top-companies-used-nodejs-production)，以帮助您了解这可以帮助解决的业务挑战的类型。

为什么要用 Lambda？

### **Lambda(AWS)** 是一种计算服务，让你无需配置或管理服务器就能运行代码。AWS Lambda 仅在需要时执行您的代码，并自动伸缩，从每天几个请求到每秒几千个请求。

那么，问题在哪里呢？好吧，没有陷阱！

使用 Lambdas 是完全透明的——你只需为被触发的功能付费:

每月前 100 万次请求是免费的

*   每 100 万次请求收取 0.20 美元，超过该金额的每次请求收取 0.0000002 美元
*   Lambda 让您可以轻松扩展您的功能——您的团队在确保持续交付和集成方面不会有任何问题。如果你需要更多的资源，你可以从亚马逊 AWS 购买更多。

我们利用 AWS 的功能帮助首批银行即平台公司之一在金融科技领域取得突破—[了解 solarisBank 是如何做到的](/web/20221209121003/https://www.netguru.com/featured/solarisbank)以及您的公司如何从无前期成本中获益。

We helped one of the first Banking-as-a-Platform companies break through in FinTech by taking advantage of AWS features – [find out how solarisBank did it](/web/20221209121003/https://www.netguru.com/featured/solarisbank) and how your company can benefit from no upfront costs.

为什么使用无服务器？

### 无服务器为构建和运行应用程序和服务提供了一个稳定的结构，没有服务器的所有麻烦。

这意味着您的系统可以自行扩展，并以更精确的方式响应需求，从而节省您的时间和金钱。

它支持最大的云提供商，例如:

AWS

*   微软 Azure
*   IBM OpenWhisk
*   JavaScript、Python、Java 等多种语言
*   我们整理了一篇关于[无服务器化如何影响应用程序开发过程的文章](/web/20221209121003/https://www.netguru.com/blog/serverless-development-the-future-looks-bright)来帮助你做出决定。

如果你想了解更多关于[无服务器化的好处](/web/20221209121003/https://www.netguru.com/blog/what-are-the-benefits-of-serverless)请查看本指南。

完美的搭配

### 将这三种工具结合起来，你就可以在一个平台上创建灵活、快速和轻量级的特性。

不需要担心复杂性，因为 Lambda 有一个自动重定功能——Lambda 可以处理几乎无限量的请求，并对所有请求做出响应。

通过无服务器，我们获得了在一个地方构建新功能的工具，并且可以将无服务器直接连接到 亚马逊网络服务——不必担心手动设置 AWS，您将在无服务器框架中找到您需要的一切。

我们的团队是 AWS 咨询合作伙伴，可以帮助你开始使用这些工具来创建一个完美的、轻量级的网络应用。

Our team are [AWS Consultancy Partners](/web/20221209121003/https://www.netguru.com/services/aws-services) and can help you get started with these tools to create a perfect, and lightweight, web application.