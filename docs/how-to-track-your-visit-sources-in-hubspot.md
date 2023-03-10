# 如何在 HubSpot 追踪你的访问来源

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-to-track-your-visit-sources-in-hubspot>

 网上营销可能真的很复杂。你必须管理许多广告网络的账户，管理你的数据库，以及设置集成、工作流程和电子邮件活动。你的客户来自各个地方——在线和离线广告、时事通讯、再销售广告、推荐网站和有机搜索。正确追踪所有这些来源会让你非常头疼。如何让它变得更简单？

HubSpot 的目标是自动跟踪所有的来源，但它有自己的怪癖，在开始时可能不是很清楚(至少对我们来说不是)。付费搜索充满了反模式，可能是最令人困惑的。

## 付费搜索

首先，**HubSpot 中的付费搜索是带有“adwords”、“ppc”或“CPC”UTM _ source 标签的一切**。脸书的广告和横幅广告通常被标记为 PPC，你会把它们放入 HubSpot 的其他活动中，对吗？不，**你必须在这里看付费搜索**。

HubSpot 的另一个有趣的“特性”是它的表[详细的 URL 和访问的推荐规则](http://web.archive.org/web/20220925213000/https://knowledge.hubspot.com/articles/KCS_Article/Reports/How-does-HubSpot-categorize-visits-contacts-and-customers-in-the-Sources-Report)。匹配的第一个规则将决定访问的来源。让我们看看哪个规则刚好在付费搜索之上:

**如果 utm_source 或 utm_medium 或 utm_campaign 或 source 包含单词“email”，则将 source 设置为 Email marketing。**

现在，尝试将您的 Adwords utm_campaign 标记为`best email templates`。

这还不是全部。**付费搜索失去你的 utm_medium 标签。** HubSpot 在保存联系人源时会将其删除。不过，它保存了关键字/术语。在为我们的广告设置 utm_medium 时，我们在跟踪方面遇到了一个小问题，我们最终复制了 utm_medium 和 utm_term，以便在分析中保持良好的排序，但也不会在 HS 中丢失信息。

## 归因报告

让谷歌分析如此强大的功能之一是它的属性报告。通常转换不能只归因于一个渠道，GA 基本上告诉你哪些渠道是有贡献的，所以你可以在它们之间划分转换值。

HubSpot 不支持属性报告。如果您的联系人的第一次访问来自 organic search，但他们后来因为您的再营销广告而直接转化，您将无法在报告中看到这一点。只有第一个就诊来源保存在“原始来源”字段，后续就诊被忽略。

然而，**如果你在所有链接中使用 UTM 标签，你可以手动检查这些访问的 URL。只需将鼠标悬停在联系人时间线中的页面名称上，并查看目的地 URL(屏幕左下角)。你可能会问“为什么不自动化呢？只要从再销售广告中创建一个访问过的人的列表。不完全是这样——创建一个包含“UTM _ source = Facebook _ remarketing”等页面浏览 URL 的列表(见下图)不会返回任何结果，即使您的联系人通过该来源访问您。**

![Screen_Shot_2016-04-21_at_12.14.21.png](img/8eec8f867e5ba677be79bed5c0e0e7a1.png)

我真的没有寻找这个问题的解决方案——我们能够在再销售工具中跟踪转换。这并不方便，但是可行，而且我们有所有的数据。我猜想这可以通过从 URL 中提取 utm 标签并将其作为 HubSpot 事件发送，使用 HubL 和模板中的一些自定义代码来完成。不过，这需要企业计划，所以对您来说可能不是一个可行的解决方案。

## 额外收获:离线追踪

与其他营销人员交谈时，我总是惊讶于他们中的许多人不知道如何跟踪线下资源，如海报、传单、横幅等。这其实很简单，但是需要一点技巧。

以 Netguru 网站为例。我们想知道我们在招聘会上的出现是否会导致我们的[netguru.com/career](/web/20220925213000/https://www.netguru.com/career)页面上有更多的申请。我们向参与者分发传单，并张贴一张印有 netguru.com/career 网址的海报。在 HubSpot，我们唯一能看到的是直接访问的增加——所以我们不确定哪些是这次招聘会的结果。解决办法很简单:在营销材料上使用 netguru.com/career。

[Netguru.com/career](/web/20220925213000/https://www.netguru.com/career)将重定向至`netguru.com/career?utm_source=offline&utm_medium=leaflet&utm_campaign=job_affair_XYZ`。轻松点。也适用于所有其他营销工具:)

我希望这些技巧能对所有使用 HubSpot 的人有所帮助。如果你知道更多，请告诉我！更多类似的帖子将很快出现- [注册时事通讯，保持联系](/web/20220925213000/https://www.netguru.com/newsletter/codestories-european-tech-newsletter)。