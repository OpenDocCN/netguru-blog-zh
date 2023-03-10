# 用 Jest 框架进行完美的 React/Redux 测试——第 1 部分

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/flawless-react/redux-testing-with-the-jest-framework-part-1>

 测试 React/Redux 应用程序可能是一个问题。最近，随着每天都有新的工具被创造出来，选择合适的工具变得比以往更加困难。在本文中，我将向您展示如何使用 Jest 来完成您的测试。

先简单介绍一下什么是 Jest 吧。Jest 是由脸书开发的 Javascript 测试框架。它是在 Jasmine 的基础上做了一些很大的改进。一些花哨的笑话功能有:

*   内置支持里面有 jsdom 的 DOM，
*   集成的人工模拟库，
*   支持测试异步代码，
*   观看模式！–仅对发生更改的文件运行测试，
*   自动覆盖报告的创建，
*   快照测试–创建生成内容的快照并进行比较，以简化 UI 测试。

要开始使用 Jest ，你可以下载 create-react-app 包，这是一种引导新的个人项目的便捷方式。

$ NPM install-g create react-app
$ create-react-app net guru-react-redux-1
$ CD net guru-react-redux-1&&NPM start

现在，您可以使用以下命令在观察模式下启动您的测试:

$ npm test

在引擎盖下运行:

$ jest --watch

在每个 React/Redux 项目中，你至少需要创建和测试四样东西:reducer、actions、component、container。在本教程中，我们将看看前两个。

假设我们有一个从远程服务器加载报价的应用程序。我们的减速器看起来如下:

[代码]

测试减速器非常简单。这里没有魔法。

[代码]

**提示:**我们使用等同于而不是等同于，因为我们需要通过值来比较对象。 toEqual 执行深度相等，因此它对于对象或数组是必需的。

检查 reducer 是否输出了期望的初始状态，并检查一旦分派了未知动作，它是否返回了当前状态，这是一个很好的实践。换句话说，当我们在状态树的其他部分执行一个动作时，你更希望检查它没有丢失状态。相信我，这个简单的检查有时可以节省几个小时的调试时间。

我们的动作也很基础。我们一执行它，它就调度 FETCH_QUOTES_REQUEST，，这样我们就可以用它来显示，比如一个装载指示器。

接下来，它调用 API，并根据响应，调度带有数据的 FETCH_QUOTES_SUCCESS 或 FETCH_QUOTES_FAILURE 。

为了异步调度 redux 动作，除了使用 redux ，我们还需要使用 redux-thunk 。这里有一件重要的事情:我们需要从我们的行动中返回承诺，以便 Jest 知道如何处理它并等待它解决。

[代码]

这里的事情越来越有趣了。我们需要测试两个场景:(1)服务器用一个有效的响应进行响应，以及(2)服务器用一个错误进行响应。我们使用fetch-mock包来模拟服务器响应，它有一个非常简单的 API。我们还需要 redux-mock-store ，它提供了一个有用的方法来检查分派的动作。类似于我们在动作中所做的，我们也需要在这里返回承诺来让 Jest 知道它应该如何处理它。

[代码]

请记住，我们需要调用fetch mock . restore()；方法来重置 fetch mock，并在下一个测试用例中为我们的请求使用不同的响应。最好在每个测试用例之后运行的函数之后的中进行这种重置——这样，我们的测试将保持整洁。

敬请关注我们未来的帖子！我们将测试应用程序的 React 部分，并使用更多 Jest 的功能来创建一个完整的测试套件。