# Ruby Brief #43 -谷歌应用引擎、AWS Lambda 和支付系统

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/ruby-brief-43-google-app-engine-aws-lambda-payment-systems>

 欢迎来到 Ruby Brief 的四月号。三月给我们带来了一些值得探索的好内容，让我们开门见山吧！

我花了相当长的时间，但谷歌终于通过 dockerized 容器为其云计算平台添加了 Ruby 支持。值得一提的是，一切都是开源的。你可以在这里 阅读指南和操作指南 [，并在 Github](http://web.archive.org/web/20221209120530/https://cloud.google.com/appengine/docs/flexible/ruby/)[repo](http://web.archive.org/web/20221209120530/https://github.com/GoogleCloudPlatform/ruby-docker)中深入研究源代码。

* * *

Truffleruby 是 Oracle 开发的 ruby 高性能实现，最开始是 JRuby 的一个分支。虽然还处于起步阶段，但 Truffleruby 绝对是一个值得在 2017 年晚些时候关注和检验的项目。 [阅读更多](http://web.archive.org/web/20221209120530/https://github.com/graalvm/truffleruby)

* * *

Thiago Araújo Silva 分享并解释了一些良好的编码实践，揭穿了神话，并展示了针对您迟早会遇到的各种开发和性能问题的几种平滑解决方案的用例。 [阅读更多](http://web.archive.org/web/20221209120530/https://blog.codeminer42.com/towards-minimal-idiomatic-and-performant-ruby-code-f3fc6aed3c94)

* * *

### 网络研讨会:支付与 Stripe 和 Taxamo 的集成

【2017 年 4 月 6 日，Netguru 将为所有 的人举办一场网络研讨会

![lazy](img/1d7b525aab91d64a86ca969343418922.png)are interested in payment integrations. Our developer, Grzegorz Unijewski, will show you the ins and outs of implementing payments into your Rails apps.

* * *

Brandon Hilkert 的高级 Sidekiq 技巧。Brandon 使用 AWS 基础设施来监控 Sidekiq 的队列数据——给定时间内的重试次数、失败的作业和总体性能。在应用程序负载显著增加并需要扩展之前，监控会给他一个提醒。 [阅读更多](http://web.archive.org/web/20221209120530/http://brandonhilkert.com/blog/monitoring-sidekiq-using-aws-lambda-and-cloudwatch/)

* * *

*“很多 Ruby/Rails 开发者都和 Rails 结了婚。本文将展示如何开始用 Rails 约会。”* [阅读更多](http://web.archive.org/web/20221209120530/http://rubyblog.pro/2017/03/decoupling-from-rails-repository-and-use-case)

* * *

### 新发布:

### 有趣的项目:

*   [RuCaptcha](http://web.archive.org/web/20221209120530/https://github.com/huacnlee/rucaptcha) :验证码宝石无任何依赖。没有 ImageMagick，没有 RMagick。
*   [Email _ inquire](http://web.archive.org/web/20221209120530/https://github.com/maximeg/email_inquire):检查邮件中流行的错别字和一次性邮件提供商