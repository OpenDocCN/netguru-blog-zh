# active model::serializer 能为 Ember 数据 API 做什么

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/active-model-serializers-ember>

 我曾有机会编写一个在后端模型之间具有深度嵌套关系的应用程序，在这篇文章中，我将给出一些例子来说明为什么 JSON API 是这样一个应用程序的最佳选择。

![](img/abedba4f38c669faec70ccb92c6f3212.png)

我使用 Ember 已经快一年了。我喜欢约定胜于配置的方法——它使得探索一起使用 Ember 和 Rails 所创造的可能性变得容易。我曾有机会编写一个在后端模型之间具有深度嵌套关系的应用程序，在这篇文章中，我将给出一些例子来说明为什么 JSON API 是这样一个应用程序的最佳选择。JSON API 是一个相当新的标准，但是我有使用 ActiveModelAdapter 和 JSON API 的经验，不能过分强调 **JSON API 更容易使用**。

### 侧面装载与非侧面装载

首先，你应该考虑一下你的人际关系。侧装是什么意思？假设您有两个带序列化程序的模型，如下所示:

没有侧面加载(或者说，包括)的示例 JSON 响应**在用户控制器中将是这样的:**

有效载荷如下:

Ember 数据序列化器将把它反序列化到一个`User`记录中，如果需要的话，可以自动获取相关的注释(例如，在模板`中显式获取)。然而，这意味着对每个笔记向 API 发出一个请求，如果您有一个很大的列表，这会非常麻烦。因此，我们可以让我们的序列化程序使用侧加载，并修改控制器如下:`

 `这样，我们可以将这些对象包含在一个响应负载中。Ember data 会注意到有效负载包含其他模型的附加散列，并将自动为发送的记录创建实例。例如:

这样，我们可以减少对 API 的请求数量，并改善用户体验。你甚至可以包含像`includes: %w(notes notes.comments)`这样的嵌套关系。在没有 JSON API 的情况下使用 Active Model Serializers 0.8 时，这是不可能的——所有包含都是在 serializer 中定义的，当每个端点都有不同的序列化程序时，这是一个维护噩梦。

此外，JSON API 为每个关系处理`type`键。由于这一点，您不必担心您的名为`friends`，但地址为`users` ( `has_many :friends, class: ‘User’, foreign_key: ‘user_id’`)的关系是否会被适当地反序列化为 Ember 中的`friends`关系。最后，但同样重要的是，多态和非多态关系之间基本上没有区别——两者都存储`type`键。

然而，序列化器用一种`each`方法遍历对象的关系。确保使用`ActiveRecord`、`include`或`eager_load`方法正确地包含关系。没有它，您将会以冗长的 N+1 查询结束。然而，没有解决这些问题的灵丹妙药，不愉快的情况将不可避免地出现。

处理深层嵌套关系

假设您的应用程序中有一个更有趣的场景。在你的应用程序中有几十个模型并不罕见，所有模型都通过关系连接在一起。假设您有一家公司，该公司有许多请求，这些请求有订单、注释、客户等。——几十条(甚至几百条)记录。即使使用某种 AJAX 分页，获取几个包含嵌套很深的数据的请求也会让用户等待，从而严重影响用户体验。

### 另一方面，实际上很少一次需要所有这些数据——您可能只需要一个请求中的订单数或属于该请求的客户名称—**而不是**深层嵌套的相关对象。为了提高性能，您可以创建两种(或更多)类型的序列化程序——基本序列化程序和“侧面加载序列化程序”。在您的`show`端点中，您使用完整的端点，(侧面加载完整的关系可能是值得的)，但是在`index`端点中，您可以使用瘦端点。

假设您有如下关系:

On the other hand, it's very rare to actually need all of this data at once - you likely only need the count of orders in one request or just a customer name that belongs to that request - **not** the deeply nested related objects. To improve performance, you can create two (or more) types of serializers - the base one and the "side-loaded one". In your `show` endpoints you use full ones, (it’s probably worth it to have sideloaded the full relationships), but in `index` endpoints you can use the thin ones.

Let’s say you have relationships as follows:

![](img/79f289cf2e005dc4f77a2eb12d53d2c4.png)

在你的`Request index view`中，你根本不需要那些`LineItem` s，所以不要序列化它们:

这样，输入您的`Requests`索引路由将只为深层关系的第一级等待请求有效负载侧加载，而不会进行任何额外的数据库查询来获取每个订单行项目的 id。另一方面，这种解决方案最终会导致缓存的模型没有侧面加载的记录。如果您从上面的例子中输入`order` route，那么您的模型钩子可能在没有侧载关系的情况下解析，这是错误的，因为它们存在但没有被提取。在 ember-data 中最近的变化之后，如果您需要使用`store.findRecord`在您的模型钩子中获取侧面加载的订单记录，它将立即使用缓存的瘦订单(从侧面加载)进行解析。

在后台，ember-data 将点击您的`orders`端点以获取最新数据，如果您使用侧加载的`lineItems`关系进行响应，它将最终被重新呈现。您还可以显式地让 ember-data 从端点重新加载模型，而无需立即解析，从而确保在您的控制器中，您将同时拥有订单及其行项目，只需一个简单的`reload`参数:

看看[这篇关于 ember-data 1.13 版本](http://web.archive.org/web/20221007094001/http://emberjs.com/blog/2015/06/18/ember-data-1-13-released.html)的博文。

摘要

在本文中，我试图向您展示当您遵循 JSON API 提供的约定和标准时，使用 Rails API 是多么容易。我从 0.8 版本(不支持 JSON API)开始就一直使用默认的 ActiveModelSerializers 适配器，我认为它现在更加容易和灵活。不要忘记在每个端点中显式地加载单个模型的可能性。以前没这么容易！

### 有了这些知识，您就可以构建非常复杂的应用程序，同时在易维护性和性能之间保持平衡。然而，我很确定你已经遇到过(或者会发现)更复杂的例子。你愿意和我们分享吗？我们很想在评论中听到他们的消息！

In this post, I tried to show you how easy it can be to work with Rails API when you adhere to the conventions and standards provided by JSON API. I have been working with default ActiveModelSerializers adapters since version 0.8 (which didn’t support JSON API) and I think that it’s currently much easier and flexible. Don’t forget about the possibility of explicitly sideloading individual models in each endpoint. It was not so easy before!

With this knowledge, you can build very complex applications while still having a balance between ease of maintenance and performance. However, I am pretty sure that you’ve come across (or will find) more complex examples. Would you care to share them with us? We’d love to hear about them in the comments!`