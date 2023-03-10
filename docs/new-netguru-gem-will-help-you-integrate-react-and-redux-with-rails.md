# 新的 Netguru Gem 将帮助您将 React 和 Redux 与 Rails 集成

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/new-netguru-gem-will-help-you-integrate-react-and-redux-with-rails>

 我曾参与 react_webpack_rails gem 的开发，它提供了 rails、react 和 web back 的简单轻量级集成。 

这个 gem 允许在 Rails 应用程序之上用现代 JavaScript 栈构建丰富的用户界面。您可以在中找到关于 react_webpack_rails 的更多信息

[this github repo](http://web.archive.org/web/20220929101437/https://github.com/netguru/react_webpack_rails)

.

有许多库可以帮助您管理数据流和客户端应用程序的状态。但是 Redux 凭借其将所有数据保存在一个存储中的理念，正变得比其他公司更受欢迎。这就是为什么在我的工作中，我想把重点放在为 gem 添加 Redux 支持上。最近，[我们发布了 rwr-redux](http://web.archive.org/web/20220929101437/https://github.com/netguru/rwr-redux)——主 gem 的集成插件。

## 你为什么要用这块宝石？

rwr-redux 提供的助手允许您在 Rails 应用程序/视图的不同部分使用 redux 状态容器。您可以将多个交互式组件添加到一个页面中，并将它们与静态组件混合在一起。组件可以访问同一个存储，并具有同步状态；初始状态也可以直接从 Rails 控制器传递给存储。

关键特征

![image00-4.png](img/88f2d1fed4026645a36327099ed60536.png)

在 Rails 视图中混合和匹配组件

### 将初始状态直接从控制器传递给存储

*   react 路由器冗余支持
*   服务器端渲染支持
*   它是如何工作的？
*   react_webpack_rails 附带了一个生成器:要添加 Redux 集成，只需在安装命令中添加- redux 选项。

如果已经安装了 react_webpack_rails，请运行:

现在，您可以使用 rwr-redux 插件中实现的方法注册您的存储和容器。Store 和所有组件都必须导入并注册到 Webpack 包的入口点:react/index.js。

导入的存储必须是一个接受初始状态作为参数并返回 redux 存储对象的函数。这样，您就能够将初始数据从控制器传递到存储器。

容器需要订阅定义的存储，因此它们需要能够访问它。Redux 库推荐使用<provider>组件。这将由 rwr-redux 处理。插件，它确保所有容器组件都有可用的存储。</provider>

呈现定义的容器——使用 gem 提供的 rails 助手。在使用 redux_container 助手之前，您必须初始化 redux_store 并从控制器传递数据。

结束语

我希望[我们的解决方案](http://web.archive.org/web/20220929101437/https://github.com/netguru/rwr-redux)能帮助你在 [Ruby on Rails 应用](/web/20220929101437/https://www.netguru.com/services/ruby-on-rails-development)中使用 React 和 Redux，让编写 JavaScript 变得更有趣。

欢迎任何和所有的贡献！如果您有任何改进 react_webpack_rails 的想法，请告诉我们。如果你喜欢我们所做的，请随意与你的程序员同事分享这篇文章:)

## Closing Notes

I hope [our solution](http://web.archive.org/web/20220929101437/https://github.com/netguru/rwr-redux) will help you use React and Redux in [Ruby on Rails applications](/web/20220929101437/https://www.netguru.com/services/ruby-on-rails-development) and make writing JavaScript more fun.

Any and all contributions are welcome! Let us know if you have any ideas for improving react_webpack_rails. Feel free to share this post with your fellow coders if you like what we’ve done :)