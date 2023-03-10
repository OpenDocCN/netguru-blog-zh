# 如何实现双因素身份验证:教程

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/devise-with-2-factor-auth>

 最近，我不得不在一个 Rails 应用程序中创建一个简单的特性——双因素认证(2FA)。我开始做了一些快速的研究，看看网上有什么推荐的，并思考它如何适合我们在 Netguru 的项目。了解我如何在 Devise 中提出双因素身份认证解决方案。

我对我的搜索结果不太满意 2FA 的话题肯定没有 design 本身讨论得广泛，虽然有许多可能的选项，但只有少数几个得到了很好的描述。因此，我认为写一篇博客文章，简要总结我的研究以及我最终认定的最佳解决方案，会有所帮助。

### 使用 Twilio 和 Authy 的双因素认证设计

**假设**:你已经设计好并开始工作。你想要的只是在短信的基础上增加 2FA 来扩展它。

Authy 是一个付费产品，所以很自然地期望他们提供非常好的文档和教程。在这里你会发现一篇关于将 Authy 与你的应用程序整合的[优秀博文。](http://web.archive.org/web/20221205021113/https://www.twilio.com/blog/2016/01/two-factor-authentication-in-rails-4-with-devise-authy-and-puppies.html)

*   优点:集成只需要很少的时间——只需要几行代码就可以完成基本的设置。
*   **缺点**:它只处理 Authy，而且相当贵——每次需要短信的认证要 0.09 美元(如果用户保存了设备，就不再需要了)，每次发送短信要 0.05 美元。然而，如果你所需要的只是验证你的 MVP，那就尽快行动，稍后再支付*也很好。*

 *### 设计与双因素认证使用谷歌认证-没有短信支持

**假设**:你已经设计好并开始工作。你想要的只是在 Google Authenticator 的基础上添加 2FA 来扩展它。

**谷歌认证器**是集成双因素认证的众多可能选项之一。它是免费的，所以经常使用。你可以想象，有一块宝石。在这里你可以阅读[如何整合`devise_google_authenticator`宝石](http://web.archive.org/web/20221205021113/https://labs.asteriskinfosec.com.au/integrating-googles-2nd-factor-authentication-with-your-rails-app/)。

*   **优点**:非常容易集成。
*   缺点:不支持短信服务，而且在默认情况下，你可以随意定制。

### 采用 Twilio - SMS(可选)和 Google Auth(定制)支持的双因素身份验证设计

**假设**:你已经设计好并开始工作。你想在 SMS 和 Google Authenticator 的基础上增加 2FA 来扩展，又不想用 Authy(由于其价格原因)。下面的方法是可行的，可定制的，快速的，但是可能需要更多的调整，主要是在用户体验方面。

这个教程是我做的，旨在为你提供一个定制 2FA 的有据可查的方法。实现全部功能不会花费你超过 2-3 个小时——如果需要更长时间，或者你有不明白的地方，[请在 Twitter 上告诉我](http://web.archive.org/web/20221205021113/https://twitter.com/jakubniechcial)。

**我们将使用什么:**

你还需要一个 Twilio 账户。你可以[注册](http://web.archive.org/web/20221205021113/https://www.twilio.com/)并简单使用一个试用账户(但仅限于开发！).

### 安装双因素身份验证

首先，你需要按照[自述文件](http://web.archive.org/web/20221205021113/https://github.com/Houdini/two_factor_authentication)安装`two_factor_authentication`宝石。安装运行迁移，向您的模型添加一些必要的列，向 device`User`类添加策略。

此外，您应该将`has_one_time_password(encrypted: true)`方法添加到负责遵守 Devise 的用户类中。

看看[提交](http://web.archive.org/web/20221205021113/https://github.com/netguru/devise-2fa/commit/e5ddde360ea8b7692234319d9015b42615743840) `two_factor_authentication`安装所需的代码。

### 设置您的模型

假设您已经正确配置了 Devise，那么您必须向您的`User`模型添加几个必要的列:

*   `two_factor_enabled`作为一个简单的标志来处理特定用户的 2FA
*   这将指出用户已启用 2FA，但没有通过提供正确的代码(来自 SMS 或 Google 授权者)进行确认；
*   `phone_number`处理用户的电话号码。

看一看[这次迁徙](http://web.archive.org/web/20221205021113/https://github.com/netguru/devise-2fa/commit/74dc241fff04c9421d473f138850cf054586ea60)。

你需要定义一个方法来告诉 Devise 什么时候使用两个因子——这叫做`need_two_factor_authentication`。如果帐户已启用双因素身份验证并已确认使用(通过安装 Google Authorizer 应用程序或提供有效的确认电话号码),此方法将返回`true`。想看看例子吗？[好了](http://web.archive.org/web/20221205021113/https://github.com/netguru/devise-2fa/commit/80640c1c7f59820ebd094cf56477a2902d99d232)！

接下来，我们需要定义一些逻辑来处理`unconfirmed_two_factor`列。双因素身份验证的确认应该设置为 on -当启用状态从 false 变为 true 时，或者当启用了 2FA 身份验证的电话号码发生变化时。

让我们把它包在`ActiveModel`回调`before_save` : 中

### 扩展您的设备注册编辑视图

在这个例子中，我们将把用户编辑建立在基本设计表单的基础上。您可以在不同的视图中将这种行为扩展到其他控制器，但想法将保持不变。

首先，您必须允许“设计更新帐户”操作的参数。您还需要生成 Devise 视图，并通过添加表单输入来扩展它们。最后，记得在`routes.rb`文件的`devise_for`声明中定义`controllers`键。

这三个步骤的代码可在本报告中[获得。](http://web.archive.org/web/20221205021113/https://github.com/netguru/devise-2fa/commit/10ff4d093bd9b45f4988e2eee5a94cffec799da0)

### 处理确认流程

您可能不知道，但是双因素身份验证的整体思想是标准化的(嗯，就像 OAuth 一样)。它基本上是一个基于特定密钥/秘密/当前时间生成 N 长度代码的协议。Google Authenticator 是一个移动应用程序，允许用户通过 QR 码获取密钥(秘密仍然隐藏在您的服务器上)，并基于该密钥，每 30 秒生成一个新代码。基本上，它不需要在第一次初始化后连接到你的应用程序，谷歌认证器生成的每个代码都将在 30 秒内有效。

首先，我们将添加[一个非常基本的方法，通过它的 API](http://web.archive.org/web/20221205021113/https://github.com/netguru/devise-2fa/commit/4398e6a38a323b56988f230a8083122ed3c1945f) 渲染一个 Google 就绪的二维码(不需要 API 键)。那么，确认流程会是什么样的呢？每当用户更改设置并保存时，一个设计动作将执行`after_update_path_for`方法来重定向用户。我们将覆盖这个方法，并基于当前的`unconfirmed_two_factor`状态，将用户重定向到`root`或我们闪亮的新确认表单。 **[看它工作并检查代码](http://web.archive.org/web/20221205021113/https://github.com/netguru/devise-2fa/commit/30fdb2444a38889d7f544caf88f24bc66489186e) :**

然后，我们将在`RegistrationsController`中创建两个动作——一个用于呈现 QR 代码，一个用于从 Google Authorizer 输入代码的简单输入；第二个处理表单、验证代码并更新当前用户对象。 [这两个动作，以及它们的视图和路由器，都可以在这里找到](http://web.archive.org/web/20221205021113/https://github.com/netguru/devise-2fa/commit/0793ab591d293479f94ea1acc623adfee27c0434)。

我们如何验证用户提供的代码是有效的？`two_factor_authentication` gem 提供了一个简单的方法，叫做`authenticate_otp`，它接受参数中的代码并验证它。我们将定义并使用`User`类上的方法，如果传递的代码有效，该方法将更新模型。

[见此法此处](http://web.archive.org/web/20221205021113/https://github.com/netguru/devise-2fa/commit/713addf4ba56e655ab7dcca8a7ae3f523942701c) :

最后，但同样重要的是，我们可以对我们的应用程序进行一些调整。首先，让我们呈现一个 flash 消息，通知用户他们必须尽快确认双因素流程。让我们在标题中添加一个到用户编辑页面的链接。 [很简单吧](http://web.archive.org/web/20221205021113/https://github.com/netguru/devise-2fa/commit/283c8ed76a1b87699c478e22e1cde527de6378f5)？

### 将 Twilio 添加到此流程中

我们仍然没有使用 Twilio 或 SMS。目前，用户进行双因素身份验证的唯一方式是使用 Google Authenticator。现在，让我们添加通过 SMS 发送代码的功能，作为 Google Authenticator 的替代方案。我们将安装`twilio-ruby`——Twilio API 的 Ruby 包装器。一旦完成，我们需要做的就是覆盖`User`类中的`send_two_factor_authentication_code`方法，该类由`two_factor_authentication` gem 提供，默认为空。我们将检查`phone_number`是否存在，如果存在，则发送一条带有当前双因素代码(实时生成)的消息。 **[见完整提交](http://web.archive.org/web/20221205021113/https://github.com/netguru/devise-2fa/commit/7bb69edd14613cd3006364da649e43acf9cda886)及摘录此处:**

你注意到`rescue`区块了吗？我们没有任何电话验证，我也不太喜欢它——这相当困难，因为不同的用户以许多不同的方式输入电话号码，比如`+48 123-456-789`、`123456789`、`123 456 789`等等。因此，我们选择忽略电话号码的有效性，除非 Twilio 发送短信有问题。如果发生这种情况，我们只需呈现一个带有适当警告的 flash 消息。用户仍然可以使用 Google Authenticator，或者只是返回到编辑用户页面并更改电话号码。

### 摘要

我对双因素身份认证的快速研究表明，仍然有一些主题没有得到很好的描述。本教程不是对 2FA 策略的深入探索，而是旨在让该特性发挥作用的快速介绍。

****那教程呢？我们已经构建了两种可能处理 2FA 的方法的基本实现——通过 SMS 和通过 Google Authenticator。让它们工作并不需要太多的努力。如果你有不明白的地方，或者教程花了你超过 2-3 个小时，请告诉我！*****