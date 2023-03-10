# 如何将 Intercom 与 Ruby on Rails 集成

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-to-integrate-intercom-with-ruby-on-rails>

 对讲机是一种流行的营销和消费者沟通工具。在你的 Ruby on Rails 应用程序中集成对讲机[并不复杂，多亏了这个指南，你应该很快就能做到。](/web/20220927110528/https://www.netguru.com/services/ruby-on-rails-development)

我假设你有:

你将使用什么

I assume you have:

优点:

## 易于集成；

多功能应用实时聊天、营销自动化、客户反馈、客户支持、客户跟踪；

### 许多集成可用-脸书，Github，Mailchimp，Salesforce，Mixpanel，Stripe，Hipchat，Zendesk，Slack，Zapier，Webhooks

*   很棒的文档。

*   multipurpose - in-app live chat, marketing automation, customer feedback, customer support, client tracking;

*   缺点:

*   定价-在某些情况下可能会变得昂贵-有几个方案可供选择(实时聊天、营销自动化、客户反馈、客户支持；基于被追踪客户数量的定价。

第一步

### 从[https://www.intercom.io/](http://web.archive.org/web/20220927110528/https://www.intercom.io/)获得一个应用 id——有 14 天的试用期；但是，需要信用卡。

*   第二步

将 intercom-rails gem 添加到您的 gem 文件:

### 然后运行:

使用您的应用 id 生成配置文件:

### 第三步

### 初始化器保存为config/initializer/intercom . Rb。这是所有定制完成的地方。

记住从初始化器中移除硬编码的 APP-ID 。它应该存储在 git 忽略的配置文件中，或者作为一个环境变量。

默认情况下，内部通信脚本会添加到可以识别当前用户的每个页面上。这种行为可以通过限制特定控制器的聊天插入来改变

```
rails generate intercom:config APP-ID
```

### 或者为未识别的用户添加聊天。

第四步

```
config.app_id = ENV["INTERCOM_APP_ID"] || "sOm3th1ng"
```

进一步定制。Intercom 提供了一些非常有趣的选项。

定义当前用户的方法可以通过以下方式与内部通信关联:

```
skip_after_action :intercom_rails_auto_include
```

自定义数据可以与用户相关联:

```
config.include_for_logged_out_users = true
```

可以将用户分组，就像他们来自一个公司一样:

### 默认情况下，Intercom 会添加一个激活聊天的按钮。如果您想覆盖这种行为，请添加:

现在，每个具有 Intercom id 的元素将充当激活器。CSS 选择器的进一步定制可通过以下方式实现:

用例

对讲机可用于应用程序的四个重要领域。

```
config.user.current = Proc.new { some_method }
```

**实时聊天**

```
config.user.custom_data = {
 plan: Proc.new { |user| user.plan.name },
 happy_user: Proc.new { |user| user.happy? }
}
```

实时聊天——可以跟踪哪些用户正在访问网页，他们在做什么。最常回答的问题可以保存为模板，而消息可以自动发送(例如，当用户第一次访问特定页面或当用户第三次访问网站时)。内部通信收件箱通知客户何时输入消息以及哪个团队成员正在处理对话。

```
config.company.current = Proc.new { company_method }
```

```
config.inbox.style = :custom
```

**营销自动化**

```
config.inbox.custom_activator = '.intercom-link'
```

面向客户的自动定向电子邮件。为用户消息创建规则(例如，用户 1 个月没有登录，或者用户只使用了一次应用程序的特定部分)。个性化的内容丰富的信息-嵌入的图像、视频、按钮等。可以基于有效性创建度量(例如，70%的用户在接收到消息 A 后填写了简档页面；60%的用户在收到消息 B 后填写了个人资料页面；消息 A 表现更好)。

**客户反馈**

收集关于产品的信息(例如，在用户停止使用应用程序后请求反馈的消息；就在第一次交互之后询问关于新特征的反馈)。轻量级回复系统(拇指向上/拇指向下)，询问用户的反馈，而不会引起太多的分心。

**客户支持**

客户可以通过入站电子邮件或应用程序内的 messenger 联系。客户问题处理可以分配给在给定领域最有经验的团队成员。所有客户数据和以前的对话都存储在一个地方，以便团队始终与客户保持一致。私人笔记可以添加到任何对话中——这些笔记只对团队可见，对客户不可见。

摘要

Automated, targeted emails for customers. Creating rules for users messaging (e.g. the user didn’t log in for 1 month, or the user used a specific part of an application only once). Personalised content-rich messages - embedded images, videos, buttons, etc. Metrics can be created based on the effectiveness (e.g. 70% of users filled up profile page after receiving message A; 60% of users filled up the profile page after receiving message B; message A performs better).

**Customer Feedback**

Gathering information about a product (e.g. a message asking for feedback after the user stopped using an application; asking for feedback about the new feature just after the first interaction). Lightweight replying system (thumb up / thumb down), asking users about feedback without causing too much distraction.

**Customer Support**

Customers can get in touch via inbound email or in-app messenger. Client questions handling can be assigned to a team member with the greatest experience in a given field. All customer data and previous conversations are stored in one place so that the team is always on the same page with the customer. Private notes can be added to any conversation - these notes are only visible to the team, not the client.

### Summary

During this tutorial, you have learned how to properly integrate and customise Intercom using a gem and example uses cases how to use it in business applications. I hope you find it helpful - feel free to comment if you have questions or like to add something.