# GraphQL 和 Node.js 入门变得简单——我们如何创新我们的应用

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/getting-started-with-graphql-and-nodejs>

 GraphQL 是一项相当新的技术，它将允许您动态调整数据要求，并在保持当前堆栈的同时提高应用程序的效率。

如果您曾经想知道如何开始使用 Node.js 开发 [GraphQL](http://web.archive.org/web/20221203080153/https://graphqleditor.com/) API，那么您正在阅读正确的文章。我们将学到足够的知识来帮助你开始开发真实世界的 API。如果 GraphQL 听起来不熟悉，或者你想知道为什么它可能是在复杂应用程序中提供数据的最佳方式，我们有一篇关于它的文章[在这里](http://web.archive.org/web/20221203080153/https://www.netguru.com/blog/grapghql-vs-rest)。

### 这是我们正在建造的

在 Netguru，我们有一个名为 Profile 的应用程序，我们在将开发人员分配到项目的过程中使用它。该应用程序的主要功能是搜索具有特定技能的 Netguru 员工(如 GraphQL)，这些技能会在应用程序中定期更新，或者搜索完整的团队(如我们公司每个前端开发人员的列表)。近几个月来，Profile 在 Netguru 团队管理中的作用迅速增加，这也是制作移动版的想法的来源——以及为其创建全新 API 的想法。我们正在用 React Native 构建一个应用程序，我们选择了 Node.js 和 GraphQL 作为后端。

在下图中，你可以看到我们的 API 是如何工作的。我们将在 Netguru 中查询可用的技能，然后检查某些用户在这些技能上有多好。

![image5-916030-edited.png](img/35cabcae56a9deedec7f9f91fc99cb51.png)

您可以在上面看到的是一个动态构建的示例查询。在左边我们可以看到我们正在请求的字段，在右边是我们的 API 的响应:我们检索到的数据。我们可以查询我们的数据的其他片段，它会工作。看一看:

![image3-779313-edited.png](img/bd3aec45866c397f84a8f13cd6a56ab0.png)

这样，我们可以请求适量的数据，而不必创建无数的端点。我们就要知道它是如何做到的了，所以做好准备。

### 先决条件

在开始之前，你应该已经安装了 [Node.js](http://web.archive.org/web/20221203080153/https://nodejs.org/en/) ，最好是最新的稳定版本。

我们的项目中有 eslint，因为它可以让我们保持代码的整洁。我们包含了预提交，以避免提交没有通过 elint 测试的代码。我们将使用 es6 及其令人敬畏的功能，所以如果你不熟悉 es6，你可能想看类似于[这个](http://web.archive.org/web/20221203080153/https://www.youtube.com/watch?v=sjyJBL5fkp8&index=1&list=PL0zVEGEvSaeHJppaRLrqjeTPnCH6vw-sm)和[这个](http://web.archive.org/web/20221203080153/https://www.youtube.com/watch?v=6sQDTgOqh-I&index=3&list=PL0zVEGEvSaeHJppaRLrqjeTPnCH6vw-sm)的视频。

我们用 MongoDB 作为我们的数据库。我们将使用 Mongoose.js 来搜索相关文档。为了在线存储数据，我们将使用 MongoLab。我们需要 babel 和 webpack 将 es6 翻译成旧版本的 JavaScript，它们已经设置好了，所以我们可以马上开始用未来的 JavaScript 编码。我们的数据将来自种子，这将使用预定义的脚本填充数据库。

### 设置服务器

我们要做的第一件事是从 Github 下载教程起点并安装它的依赖项。

打开终端并键入:

git 克隆 git @ github . com:netguru/netguru-graph QL-tutorial . git

netguru-graphql 教程 cd

npm 一号

一旦我们进入应用程序文件夹并安装了依赖项，我们将需要设置数据库。我们的应用程序使用 MongoDB，这也是我们将在教程中使用的。我们将使用云数据库服务 MongoLab 在线存储我们的数据。前往 MongoLab 并创建一个帐户(这是免费的)。现在创建一个新的部署并选择以下选项:

![image8-992041-edited.png](img/5a88852d096014ff4e42b092f6473b90.png)

选择名称后，单击“创建新的 MongoDB 部署”。我们现在需要创建一个用户，因此单击新创建的部署，然后单击 Users 选项卡。使用一个您容易记住的密码，因为您需要连接到数据库。

![image2-044500-edited.png](img/d00608864a800dd21a856f07dedfef7f.png)

瞧，现在我们需要做的就是从这里复制链接:

我们马上就要用到它了！

现在，在您最喜欢的文本编辑器(我使用的是 Atom)中打开一个项目，并在服务器文件夹中创建一个名为 mongoLabLink.js 的文件，然后粘贴链接，如下图所示，并注明“export default”。确保使用您的 MongoLab 部署名称以及您的用户名和密码。如果您有一个名为 glorious-database 的部署，用户为“hamsterFeeder ”,密码为“hamster123 ”,那么您导出的链接将如下所示

导出默认的' MongoDB://hamsterFeeder:[hamster123@ds113630.mlab.com](http://web.archive.org/web/20221203080153/mailto:hamster123@ds113630.mlab.com):<你的部署 id，上图你可以看到我的 id 是 13630>/光荣-数据库'；

现在运行:

npm 开始

您应该会在控制台中看到类似这样的内容:

![image6-096711-edited.png](img/a4436a59324cf19c3acb7c8759b4d6d9.png)

我创建了一个脚本，将种子数据放在您的服务器上。如果您访问您的 MongoLab 部署，您会看到三个名为 users、skills 和 level_skills 的集合。正如您在控制台中看到的，我们还可以在端口 8000 上访问我们的 GraphiQL。GraphQL 是一个 API，用于通过提供对 graph QL 端点的访问和可视化返回数据来测试它。最重要的是，它支持自动完成，并为我们请求的字段提供描述。

教程的下一部分将讨论如何创建查询数据的方法。

现在当心！我们将体验可能是市场上最复杂的 API，GraphiQL。在浏览器中访问 localhost:8000，看看它是什么样子。

### 使用查询

在图形的左边部分键入{ helloQuery }。以下是键入过程中的查询:

![image11.png](img/3286249f724f48f53686daa4e4c08e7a.png)

您应该会看到自动文档系统在运行。不仅我们请求的数据是强类型的，而且我们还可以看到查询将返回什么类型。最重要的是，每一个领域都可以按照我们的意愿来描述，并且这种描述是可见的。查询的功能远远超出了我们在一篇文章中所能涵盖的范围，所以你可能想在这里阅读一下。

此时，我们已经定义了一个查询字段，它实际上并没有做任何有用的事情，所以我们可以真正地从头开始开发我们的模式。

我们需要为用户添加一个查询字段。GraphQL 定义了一组我们可以用来描述用户的类型。简而言之，我们可以说用户有一个字符串类型的字段 firstName。类型在官方文档中有很好的解释。如果你想更深入地了解它们是什么，你可以在这里了解它们。

我们现在将为用户创建一个 GraphQLObject 类型。要查看用户可能有哪些类型的字段，请访问 server/appData/models/user.js。我们有一个模型，它告诉您用户可以有以下字段:名字、姓氏、角色和 _level_skills。_level_skills 字段返回一个标识符数组，我们将很快处理它。现在我们只处理四个字段，即 id、名字、姓氏和角色。
这是我们的用户模型:

我们现在可以创建一个对 MongoDB 的调用来调用数据库中的所有用户。我们需要导入 GraphQLList 来实现这一点。这是我们目前的进口声明:

现在我们可以向 fields 函数添加一个新的对象参数。简单地复制这段代码:

要查看是否一切正常，请在终端中重启服务器。

运行 ctrl + c，然后 npm start。

如果有些东西不工作，不用担心，这里是我们迄今为止完整的模式:

免责声明！您可能需要在浏览器中重新加载 GraphiQL 窗口来查看新的查询字段。

现在访问 [http://localhost:8000/](http://web.archive.org/web/20221203080153/http://localhost:8000/) 。您现在可以拨打用户查询电话，查看一下！

![image9-304074-edited.png](img/200d77b1e36fa7bff3a99af071114349.png)

我们想做的是找出我们的每个用户在每个编程技能上有多好。我们还没有技能级别或技能类型的字段，所以我们还不能查询。让我们解决这个问题！

### 添加技能和技能级别类型

注意:技能水平模型有一个最喜欢的字段，它是一个布尔值，所以我们需要导入 GraphQLBoolean，就像我们导入 GraphQLList 一样。我们还需要 GraphQLInt 用于级别字段，因为我们用整数描述技能。

这是我们目前的进口货:

以下是我们的类型:

要检查我们的更改是否有效，请在终端中键入 ctrl + c，然后在浏览器中键入 npm start 并重新加载 localhost:8000。您将在可用查询中看到技能和技能水平。还要注意，我们被告知它们返回什么类型，就像这样:

![image7-370871-edited.png](img/9b52c954d2aa2707159f3f7878f8f19a.png)

我们已经准备好做我们最初打算做的事情，那就是显示分配给我们用户的技能水平。

为此，我们需要 skillLevel 字段来返回一个对象列表(技能级别)。这听起来可能令人困惑，但只要看看这个例子:

![image4-431779-edited.png](img/d8afb99a4c350a7ad9b5a27c5c3dec5f.png)

resolve 函数有几个参数，但在这种情况下，我们只关心第一个参数，它反映了传递给类型的内容。在我们的例子中，它只是我们在用户模型下获取的内容。接下来，我们找到与标识符相对应的技能水平，并将它们放在一个列表中。这样我们就有了一个技能等级对象的列表。
如果您想复制代码，看看它是如何为您工作的，以下是用户类型:

我们现在可以看到用户的技能水平。看一看。

![image13-734595-edited.png](img/0a4dcffd51b6cf1cc8b43a8da3d587e8.png)

我们可以映射我们的逻辑来搜索组织中的技能水平。如果我们需要知道我们有多少像样的 Node.js 和 GraphQL 开发人员，这将非常有用。如果你是来复制粘贴代码的，这是我们升级的技能类型:

为了能够随心所欲地查询字段，我们应该查找 skill_level 字段和评级技能的所有者。我们可以用很少的代码做到这一点。

有了这个，一切都应该很好地工作。如果您重新加载应用程序并刷新 GraphiQL，您将能够像第一个示例中一样查询我们的数据:

![image10-572508-edited.png](img/b62b03f6123c8a21196d170e87c6c47e.png)

这是我们使用 graphql-voyager 可视化的整个模式结构:

包扎

![image1-2.png](img/07b57dc7183f440764aff291daa0daac.png)

我们已经了解了 GraphQL(不要与 graph QL 混淆)如何帮助编写 API 文档。

### 我们知道 GraphQL 的强类型是如何工作的，也了解了一些内置类型。

我们可以随心所欲地创建对象、列表和查询数据。

如果您在任何地方遇到困难，您可以在 completeSchema 分支中找到完整的代码。只需提交您的更改并键入:

文本

git 签出完成模式

还有许多主题有待讨论，例如创建突变、使用惊人的数据加载器库防止 N+1 查询、在查询中使用限制、认证或使用
查询变量等参数。既然您已经了解了基础知识，那么开始学习所有这些知识应该很容易。

我们只在最容易和最有效的时候将 GraphQL 添加到现有的应用程序中。我们不会废弃或破坏已经存在并正在运行的解决方案——相反，我们会利用 GraphQL 进一步发展移动应用。因此，我们充分利用我们的资产，创造新的价值。考虑一下这对你的企业来说是不是最好的选择。

There are many subjects left to be discussed, such as creating mutations, preventing N+1 queries with an amazing data-loader library, using arguments like limits in queries, authentication or using
query variables. Now that you know the basics, getting started with learning about all of them should be easy.

We’re only adding GraphQL to existing applications when it’s the easiest and most efficient thing to do. We don’t scrap or destroy solutions that are already there and working - instead, we build up by taking the mobile app further with GraphQL. Thus, we’re taking full advantage of our assets and creating new value. Consider whether that’s not the best option for your business, too.