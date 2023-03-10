# Ember:为返回的承诺添加 Catch 块

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/ember-adding-catch-block-for-returned-promise>

 承诺是 JS 开发者工具箱中的一个有价值的工具。在他们的帮助下，我们可以避免回调地狱，并编写一些结构优美的代码。但是承诺可能非常棘手...

我是[一个 Ember](http://web.archive.org/web/20221007195506/https://www.netguru.com/services/vue-js-development) 人，我使用 Ember 提供的 promise 实现，它符合 ES6 定义。最近，我忘记了承诺的一个特点，在随之而来的困难中，我学到了一些新的东西，我想与你分享。

## 余烬遇上 API

假设您想使用模型钩子中的 Ember 数据从 API 获取一些数据:

但是你认为这有风险。您的后端人员遇到了一些问题，有时服务器在该端点没有响应。在他们修复它之前，这个模型钩子的错误会让你的应用程序陷入错误的轨道。你想确保这不会发生。

答对了！当服务器关闭时，用户被重定向到另一个路由。但是，你刚刚弄坏了 app...当服务器启动时，您的`setupController`操作将获得一个`undefined`模型。为什么？

## 应对承诺中的诡计

好的，我知道承诺是可以链接的，所以我使用了上面的方法。然而，我忘记了`catch`和`then`也是可链接的，它们返回 promise 类的**新**实例，该实例将解析为您从`then`或`catch`返回的值。我们所做的是转换但不返回任何东西，所以`model`钩子基本上是返回`undefined`。

我们被困住了吗？不，这种情况有一个简单易行的解决方法:

如您所见，我们从`store`返回了初始的 promise 对象，并在完全不同的代码行中链接了该承诺。我们不使用链式方法返回的值，现在一切都正常了！

我希望你觉得这个案例很有趣。在 Emberjs 工作期间还遇到过其他有趣的许诺情况吗？请在评论中告诉我！

如果余烬是你感兴趣的领域之一，我有一些有趣的读物给你:

[11 Ember.js 资源你一定要去看看](http://web.archive.org/web/20221007195506/https://www.netguru.com/blog/emberjs-resources)

[active model::serializer 能为 Ember 数据 API 做什么](http://web.archive.org/web/20221007195506/https://www.netguru.com/blog/active-model-serializers-ember)