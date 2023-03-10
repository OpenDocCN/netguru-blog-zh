# 你的余烬测试应该是什么样的？

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/ember-tests>

 每个生产应用都必须进行测试。在 [Ember apps](http://web.archive.org/web/20220926195208/https://www.netguru.com/services/vue-js-development) 中，测试之间的区别类似于任何其他 Rails 应用程序，但是它们的用例经常被误解。让我根据我对一个大型应用程序的经验，快速总结一下你的测试套件应该是什么样的，我花了一年多的时间进行不断的开发。

![](img/0306bd8ba9b840841ea0b841f4dc5e31.png)

每个生产应用都必须进行测试。他们让你对你正在开发的东西有信心，但更重要的是，他们确保维护和进一步的特性不会破坏业务逻辑。在 Ember apps 中，测试之间的区别类似于任何其他 Rails 应用程序——验收、单元和[集成测试](/web/20220926195208/https://www.netguru.com/blog/e2e-testing-vs-integration-testing)。但是它们的用例经常被误解。让我根据我对一个大型应用程序的经验，快速总结一下你的测试套件应该是什么样的，我花了一年多的时间进行不断的开发。

## 验收测试

许多初学者犯的第一个错误是将他们的信心建立在验收测试上。为什么？原因很简单——我们大多数人一开始都觉得它们最容易。您使用选择器作为参考，执行用户操作，异步助手完成所有关于 Ember 运行循环的工作。但是我强烈建议你不要坚持把接受作为最重要的测试类型。首先，与单元测试和集成测试相比，它们非常慢。它们要求应用程序在每次测试后重新加载。而且他们只测试你正在访问的页面，不测试页面上使用的和其他页面可能会用到的组件(自己重复就不好了！).即使验收测试看起来像是一个快速的胜利，让它们成为你的测试套件的一大部分实际上是一笔技术债务，你将不得不在未来偿还。

你可能会发现技术讨债人以缓慢测试套件的形式来敲你的门。如果是这样，不要删除你的测试(这也是一个巨大的错误——它们很好，但是很慢)。尝试重构它们以使用一些页面对象([参见](http://web.archive.org/web/20220926195208/http://confreaks.tv/videos/wickedgoodember2015-compose-all-the-things)[林里德](http://web.archive.org/web/20220926195208/http://twitter.com/linstula)的精彩演讲)，并通过将它们合并到用户路径中来最小化测试用例。与将邮件放入收件箱的测试——分别留下、存档和搜索——不同的是，只需在一个测试用例中按顺序执行这些任务。每次重新加载应用程序都会占用你几秒钟的时间。

## 单元测试

单元测试对于初学者来说有点难理解，但是最终必须在你的工具带中找到一个位置。它们速度极快，非常适合测试路线和控制器。然而，初学者的第一个障碍是如何使用 ember-data 和您在控制器或路由中注入的其他服务。实际上，我强烈建议你尽可能地嘲笑一切。使用与真实数据几乎相同的数据测试您的方法应该执行的逻辑路径。例如，您不需要在单元测试中使用 ember 数据来创建记录。您可以模仿`store`来返回与您的模型具有相同属性的`Ember.Object`:

集成测试

## 这仍然没有被开发人员广泛使用，因为它是一个相当新的测试选项。然而，我发现它对于测试您的组件非常有趣和有用。您可以轻松地在测试范围内呈现组件，测试用户行为，并像使用单元测试一样快速地完成组件所做的一切。此外，如果你像在单元测试中那样模仿一切，那么使用它们会非常容易。不要害怕 Ember Run 循环，因为集成测试会对你隐藏它，并避免你遇到任何问题！集成测试可以在[的 Alisdair McDiarmid](http://web.archive.org/web/20220926195208/http://alisdair.mcdiarmid.org/) 的这篇优秀博文中查看。不幸的是，它们在 Ember 指南中没有被恰当地记录——它们将验收测试作为集成测试。这是误导，所以我建议你坚持上面的博文。

端到端测试

## 在和恩伯一起工作的时候。JS，我发现单独测试你的 API 和 Ember 应用程序并不能给你足够的信心，当你随后修改代码时，可能会导致应用程序崩溃。如果你在验收测试中模仿你的 API，你永远不能确定模仿是否准确地反映了真实 API 的行为。因此，这两个应用程序的绿灯并不意味着您在这两个应用程序上测试的特性最终可以工作。这让我陷入了极度紧张的境地，所以我建议你一定要避免这种情况。端到端测试对我来说是相对较新的东西，所以我不能说我们的过程是完美的——我们仍然需要几个星期的测试。我打算在几周内写一篇关于我们的设置的博文，但是现在，你可以查看船坞关于他们的方法的[这篇文章。](http://web.archive.org/web/20220926195208/https://dockyard.com/blog/2015/03/25/testing-when-your-frontend-and-backend-are-separated)

摘要

## 总之，我想再次强调，测试是 Ember 应用程序的重要组成部分。您应该尝试划分您的测试套件，使其尽可能快，并提供足够的信心来处理代码。在单元和集成测试中利用嘲讽，不要完全依赖验收测试。如果你的应用是一个商业项目，不要忘记端到端的测试——制作可靠的软件。

In summary, I would like to stress again that testing is an essential part of Ember applications. You should try to divide your test suite to make it both as fast as possible and provide enough confidence to work with the code. Take advantage of mocking during unit and integration tests and don’t rely fully on acceptance testing. If your app is a commercial project, don’t forget about end-to-end testing - make software that is reliable.