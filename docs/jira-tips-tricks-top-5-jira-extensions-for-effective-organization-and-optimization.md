# 吉拉提示和技巧:有效组织和优化的 5 大吉拉扩展

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/jira-tips-tricks-top-5-jira-extensions-for-effective-organization-and-optimization>

 吉拉是一个非常受欢迎的工具，用于跟踪 bug、问题和项目管理活动，所以你会认为它提供的广泛选项在这一点上只是常识，生产集成和应用程序的采用已经被利用到了很难再做更有效的事情的地步。然而，现实有所不同，大多数公司没有充分利用吉拉的潜力有几个原因，主要是:

*   如果不花几个月的时间深入研究配置选项，就无法掌握内置的吉拉功能，
*   大量可用的集成可以显著扩展内置功能，这使得很难选择最适合我们需求的集成。

好消息是，Atlassian 正在努力通过他们的新“下一代”项目使吉拉变得更加用户友好，我们相信这将在未来为新的和不太懂技术的用户减轻复杂配置的痛苦。此时，在 2019 年 3 月，“下一代”配置项目还不适合更复杂的开发计划，因为它们在工作流定制方面存在限制，无法同时拥有多个板，或者缺乏基于以前团队速度的自动路线图预测。也就是说，我们期待着这一易于使用的概念的下一次迭代，因为它已经在我们组织的多个项目中带来了价值。

当谈到各种可用的吉拉集成和应用程序时，希望这篇文章能派上用场，并通过向您提供我们目前在 Netguru 每天使用并从中受益的 5 个最重要的吉拉集成，让您了解组织改进的可能性。

### 1.希拉-斯拉克

Atlassian 前段时间宣布与 Slack 结成战略合作伙伴关系，同时通知用户，他们将在未来停止支持 Hipchat 和 Stride。这表明 Atlassian 是多么相信 Slack 是一种面向未来的企业通信工具。尽管自从我们开始使用 Slack 以来，我们确实遇到了一些问题，但我们完全可以推荐它，所以如果你还没有使用过它，它可能值得一试。与吉拉的集成设计得非常好，也很容易配置。

![](img/4a1b1844c7f05f0ee6057be01d5f9cfe.png)

那么使用吉拉-斯莱克集成有什么好处呢？有两个主要的工具可以使不同项目的协作更加有效:票据预览和通知。

当您的工作流程涉及票证生命周期不同阶段的不同人员协作时，第一种方法非常有用，而另一种方法可让您了解吉拉的最新变化，而无需一直监控。当人们在 Slack 上讨论特定的吉拉门票时(例如，当质量保证专家检查门票并向开发人员提出其他问题时)，他们可能会通过使用其密钥来引用它，在集成打开的情况下，这将导致吉拉云 Slack 应用在对话中发布门票的清晰预览，以便快速识别门票。在这种情况下，您会看到这样的情况:

它节省了相关各方的宝贵时间，否则他们将不得不花费在打开吉拉和使用侧边栏搜索工具来定位任务和阅读其描述上。这并不能节省大量的时间，但是它的积极影响会随着组织的规模而增长。

吉拉云提供的通知可以针对不同的吉拉事件进行配置(如票据创建或转换)。对于较大的项目，在单独的 Slack 通道上配置它们可能是一个好主意，以避免在一堆自动生成的通知中丢失人类对话的线索。这些通知可能如下所示:

![](img/8e01b5bbfa6531d2a3b120738d2c6741.png)

**2。吉拉——GitHub**

第二个重要的集成是吉拉-Github 集成，通过在开发过程中节省一点时间，你可以更好地组织你的工作。它能提供什么，如何节省您的时间？它只允许您的团队直接从吉拉的票证视图中查看他们的分支、提交消息和拉取请求。这样，当一个标签处于“代码审查”阶段时，例如，另一个团队成员可以通过进入标签的详细信息并点击开发部分来快速查看其代码，只要他们具有名为“查看开发工具”的吉拉权限。那个特殊的部分看起来像这样:

![](img/b5a44400e6558c69a2f66999432274b2.png)

### 另一个重要的方面是，当一个拉请求被合并到 Github 上时，吉拉允许你自动执行票据转换。这是一个好主意，以一种能让你从中受益的方式配置你的吉拉，因为它真的能节省你的人的时间。利用这一点的一个方法是在 QA 检查并接受票据后合并拉请求。在这种情况下，您可以在项目的工作流程中添加 post 功能，根据您在项目中使用的环境设置和您对“完成”的定义，在吉拉自动将票证从“QA”列转换为“已合并”或“完成”。想到的一个问题是，吉拉如何知道哪个合并的拉请求应该触发特定票据的转换。这是通过匹配吉拉票证发行密钥和应该包含该发行密钥的分支名称来实现的。命名分支的实践必须遵循，这样集成才能完美地工作。

由于集成的“智能提交”功能，您团队中的开发人员甚至可以使用这种集成来留下评论、记录在票证上花费的时间或更新其状态，而无需离开命令行或 GitHub。

![](img/e21dbdcec86a9cd3629b99e0c37c42c2.png)

我们 iDalko 的下一个提议不是集成，而是我们在组织中以多种方式使用的吉拉应用程序。这是什么？Exalate 被宣传为最灵活的问题跟踪同步工具，从我们目前的观察来看，这个简短的定义相当准确。它允许您配置可定制的连接，使不同的项目或问题保持同步(即使项目存储在不同的吉拉环境中也是可行的)。很难想象用它能实现什么，所以让我给你两个关于 Netguru 如何利用 Exalate 的场景。

![](img/1df17d4ca0b54627720ae1db3be9e47d.png)

Netguru 有一个内部 DevOps 团队，他们在自己的内部吉拉服务台工作，但我们经常希望 DevOps 工程师处理与我们的商业项目相关的案例。在这种情况下，我们希望他们的工作得到更新，并反映在我们的商业项目板上。Exalate 如何支持这种场景？我们配置了一个 Exalate 连接和一个触发器来实现这一点。触发器监听我们所有项目上的“标签”字段的内容，并且每当它注意到一个“Devops-Synchronization”标签已经被添加到一个商业项目票据上时，它就启动一个连接。另一方面，连接负责在 DevOps 服务台上创建特定票据的映射(它不必是一对一的副本),在那里，适当的人负责其估计、分配和执行。Exalate 使两张票保持同步。您甚至可以配置哪些字段应该同步以及如何同步，这样商业项目和 DevOps 团队就可以随时了解他们要做的事情以及他们当前的工作量。这有助于他们更有效地利用资源和协作。

Netguru 营销团队希望开始使用吉拉作为任务跟踪和报告解决方案。作为一个团队，他们每天都会收到来自不同团队成员的大量请求，因此利用吉拉服务台界面来创建票证似乎是个不错的主意。问题是，营销团队的规模加上吉拉服务台的每个代理计费规则大大增加了我们每月的 Atlassian 订阅成本，我们觉得有必要进行优化。我们做了什么？我们已经在 Netguru 上使用 Exalate，所以我们想出了一种方法来利用它。我们创建了一个额外的吉拉项目(不是服务台)，在营销服务台工作流中配置了一个新的中间件云服务器触发器、一个连接和一个额外的 post 功能。它是如何工作的？触发器监听“标签”字段，每当它看到一个名为“GSD-通用汽车”的标签时，它就会启动营销服务台和营销吉拉项目之间的连接，并在后者上创建一个同步副本，该标签是在通过营销服务台侧的 post 功能创建票证后自动添加的。这样，我们不需要所有营销团队成员都作为营销服务台的代理。他们可以从标准的吉拉项目开始工作，他们的利益相关者仍然可以使用为吉拉服务台保留的漂亮的票证创建界面。配置后的这一机制使我们能够将营销服务台的每月成本降低 95%左右。有些人可能会说，好吧，但你只是将费用从吉拉服务台订阅转移到了吉拉软件订阅，但这不是这里的情况，因为此时所有 Netguru 员工都已经默认拥有了吉拉软件席位。

**Exalate usage scenario 1:**

4.扎皮尔

**Exalate usage scenario 2:**

我们在吉拉提高生产力的下一个选择工具是 Zapier，一个拥有巨大自动化能力和易用界面的工具。你并不真的需要任何技术背景来使用 Zapier 获得好的结果。我们如何从中受益？在 Netguru，不同的团队以不同的方式使用 Zapier，但在这种情况下，让我们关注一下我们是如何将其与吉拉相结合的。我们使用员工通过吉拉服务台票证提供的数据来更新 Google Sheets 文档，该文档由处理公司开发预算的团队使用。换句话说，每当有人想参观一个有趣的会议或从他们的发展预算中购买一本书时，他们只需在某个特定的吉拉服务台处订票。如果他们的票被合适的人接受，Zapier 会负责用一个包含所提供数据的新行来更新 Google Sheets。这听起来可能很复杂，但实际上运作良好，我们的员工已经习惯于与服务台合作。直接在吉拉存储与所有员工预算相关的所有数据并不是最好的选择，尤其是如果您需要经常用精确的计算来更新值。在这里使用 Zapier 让我们能够在吉拉服务台提供的易用界面和安全存储的谷歌表单文档上进行的复杂计算之间搭建一座半自动的桥梁。

### 5.吉拉杂项工作流扩展(JMWE)

这是一个真正的宝石，它允许你在吉拉的任务中执行重要的自动化程序。它为您的工作流程添加了 20 多个额外的 post 功能，由于 Nunjucks 的注释处理，您可以通过精心设计的布局、良好的文档和出色的控制水平来实现真正的创造性配置。我们如何利用它来提高效率？让我给你举几个例子。

![](img/ca322345de985448dd10353660ca1197.png)

当我们需要根据服务台客户提供的数据将特定服务台上的一种请求类型的票证转发给不同的受分配人时，我们会配置一个具有条件执行的“设置字段值”发布功能。在我们的案例中，我们有一个帮助台，客户在服务台门户上创建票证时需要从下拉字段中选择“城市”,根据用户选择的城市，票证将被分配给在该特定城市处理请求的任何人。

我们还使用“设置字段值”发布功能，通过权衡用户在创建票证时从下拉字段中选择的答案，自动计算服务台上不同任务的优先级和影响字段。由于这一点，我们的服务台团队知道他们应该以多快的速度响应特定的票证，以及他们有多少时间来解决问题，因为我们将所有这些与 SLA 计算相结合。

### 这是我们决定在这种情况下讨论的最后一个问题。本文中提到的应用和集成对我们来说非常有用，所以在这里推荐它们是很自然的。谁知道呢，也许是因为将来会有更多的人从中受益？也就是说，这并不意味着没有更好的解决方案。这完全取决于您的需求、设置和技能，因为不同的解决方案需要不同的方法。如果你知道比这篇文章中提到的更好的选择，或者有任何与这个特定主题相关的问题，请随时与我们联系。

This one is a real gem that allows you to perform significant automation procedures on tasks in Jira. It adds more than 20 additional post functions to your workflows and you can get really creative with how to configure them thanks to a well-designed layout, good documentation and a great level of control that is achievable thanks to Nunjucks annotation handling. How do we use it to be more productive? Let me give you a few examples.

1.  We configure a “set field value” post function with a conditional execution when we need tickets of one request type on a particular Service Desk to be forwarded to different assignees depending on the data provided by the Service Desk Customers. In our case, we have a Helpdesk where customers need to choose “City” from a dropdown field when creating a ticket on the Service Desk portal and, depending on the city selected by the user, the ticket gets assigned to whoever handles requests in that particular city.
2.  We also use the “set field value” post function to automatically calculate the priority and impact fields of different tasks on our Service Desks by weighing answers selected by the users from dropdown fields when creating a ticket. Thanks to this, our Service Desk teams know how fast they should respond to a particular ticket and how much time they have to resolve the issue as we combined all this with an SLA calculation.

That was the last one we decided to cover on this occasion. Apps and integrations mentioned in this article turned out to work for us really well, so it feels natural to recommend them here. Who knows, maybe thanks to that more people can benefit from them in the future? That said, it does not mean that there are no better solutions out there. It all depends on your needs, setup, and skills, as different solutions require different means. If you know any better options than those mentioned in this article or have any questions related to this particular topic, feel free to get in touch with us.