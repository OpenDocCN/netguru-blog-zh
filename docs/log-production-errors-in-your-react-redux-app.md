# 在 React-Redux 应用程序中记录生产错误，并在开发时再现它们

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/log-production-errors-in-your-react-redux-app>

 你还记得在你的后端和前端 JavaScript 栈中集成错误报告工具吗？利用 redux 单个应用程序状态树，用完整的应用程序状态记录用户错误。使您的单页应用程序更可靠，更易于维护。

## 不要忘记前端

由于 Ember.js 等框架和 [React](http://web.archive.org/web/20221226111956/https://www.netguru.com/services/react-js) 等库的快速增长，网络应用越来越基于前端。我认为每个后端开发人员都在使用某种错误报告工具，比如[滚动条](http://web.archive.org/web/20221226111956/https://rollbar.com/)或[哨兵](http://web.archive.org/web/20221226111956/https://getsentry.com/welcome/)——但是你想过将这些工具集成到你的 JavaScript 堆栈中吗？

如果您使用 Redux，您可以利用它的单个应用程序状态树。让我给你介绍一下 redux-roll bar-中间件。如果出现错误，该工具会自动将完整的应用程序状态和动作名称及其参数记录到云中。然后，您可以立即重现您的用户在开发站上遇到错误时的情况。很酷吧。

## Redux-rollbar-middleware 是如何构建的？

Redux-rollbar-middleware 作为 Redux 的中间件。简单来说，中间件就是当新的 redux 动作发生时执行的普通函数，接受一个  下一个函数作为参数，允许执行一些操作并最终执行和返回下一个传递的。我通过运行 try {,利用了这个简单的机制...}接住{...}大约在下一个执行。

如果在执行过程中发生任何错误，无论是在 reducer 中还是在呈现组件时(这是一个执行的同步链)，我的 catch 块都会执行。然后，Redux-rollbar-middleware 将读取完整的应用程序状态，将其字符串化到 JSON，并通过一个异常对象将其发送到 rollbar。一个非常简单，但功能强大的机制，大大改善了应用程序的维护。

## 有趣的事实

想知道一些关于开发过程的有趣事实吗？我在 Netguru 的团队务虚会黑客马拉松上用了不到 6 个小时创建了这个开源软件。它基本上有 46 行代码，但我仍然认为它是最重要的依赖项之一，您应该在使用 Rollbar 时将它包含在 React-Redux 应用程序中。

使用 Ember.js？想了解更多关于如何在 Ember.js 应用程序中使用 Rollbar 的信息吗？查看我关于 Ember 应用程序部署过程的博文，使用它并利用我几个月前创建的 [ember-cli-deploy-rollbar](http://web.archive.org/web/20221226111956/https://github.com/netguru/) ！