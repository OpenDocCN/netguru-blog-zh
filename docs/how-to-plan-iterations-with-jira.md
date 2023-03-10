# 如何与吉拉一起规划迭代？

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-to-plan-iterations-with-jira>

 作为 Netguru 的项目经理，我们的职责之一是帮助我们的团队规划和跟踪开发进度。我们坚信，我们使用的工具应该促进我们的过程和需求，而不是相反。这就是我们选择 JIRA 作为项目管理工具之一的原因。

JIRA 涵盖了许多不同的项目类型和工作流程，可以进行定制，以适应我们在每个项目中单独的目的。

每个项目都需要规划。毕竟，没有计划就是计划失败。幸运的是，JIRA 帮助我们在这方面非常有效。

**规划一次迭代**  

在讨论迭代的规划时，我们可以看两个主要方面——第一个是在我们开始工作之前规划下一个 sprint(迭代)。第二个是计划发布。我们先来看看冲刺规划。

在[敏捷框架](/web/20221001235740/https://www.netguru.com/blog/agile-also-means-planning)中，冲刺可以持续一到四周。长度是根据项目中的流动和动力来选择的。如果我们需要更频繁地回顾我们的目标，更密切地控制进展，我们选择一周冲刺。如果我们在每次迭代中有更多的覆盖面，我们可以进行两周的冲刺。我们可以在 backlog 的基础上一次创建许多 sprints，如果需要的话，可以计划更长的时间。
团队需要跨职能，因此他们通常包括不同领域的多名专家——后端、前端、设计、质量保证。
其中一个很棒的功能是快速过滤，这有助于我们筛选票证，以便找到我们希望在下一次 sprint 中处理的票证。这些过滤器可以高度定制，我们可以选择按受让人、我们输入的任何标签(一些最流行的当然是描述后端、前端、移动或技术名称的标签)以及许多其他内容进行过滤。

![Screen Shot 2019-04-01 at 11.16.19](img/56bd75f102a628cba7caea255f3f29cf.png)

新的 JIRA 视图还提供了从待办事项或活动板中快速访问这些过滤器的功能——无需更多搜索，只需点击一下就能过滤掉:

![Screen Shot 2019-04-01 at 11.16.26](img/858614486347c558f892716b0417bb2a.png)

在计划时，我们在票证中使用“就绪”的定义。这意味着，在一个问题被添加到 sprint 之前，它必须包含一个用户故事和接受标准，并且必须被评估。换句话说，一个问题必须*准备好*(双关语)才能包含在 sprint 中。

我们使用不同的估算方法，JIRA 很好地解决了所有这些问题。我们现在使用的最流行的估算方法是故事点估算。这意味着，团队只考虑一个产品待定项相对于其他产品待定项需要多少努力，而不是查看一个产品待定项并以小时来估计它。一旦我们有了一些冲刺，我们就可以预测团队的速度，并据此制定计划。
但有时我们需要一个更具体的值来预测交付成本。在这种情况下，我们可以使用时间估计，JIRA 也允许我们把问题。我们甚至可以跟踪花费在每个问题上的时间，以确保我们在每次计划下一次迭代时改进我们的估计。

![Screen Shot 2019-04-01 at 11.16.32](img/f4748bb4250c2b4798390eaa79893cc3.png)

它的酷之处在于一个系统并不排斥另一个系统——我们可以估算故事点并测量团队速度，但同时我们可以输入一些关于每个问题所花费时间的粗略数据，以便能够为我们的客户预测时间表，并且不会使 sprint 过载。时间估计还帮助我们认识和考虑其他过程，例如同行代码审查和质量保证测试，这是我们所有开发工作流的标准。 

我们可以使用的另一个有用工具放在每期顶部的操作图标下。这些图标将帮助我们将问题相互联系起来，添加功能设计的预览视图，创建子任务或添加我们认为对我们有用的任何其他集成。你可以在 Piotr Radtke 的帖子[中阅读我们使用的一些集成。
将某些问题联系在一起(不同选项可用，如*与、跟随、被跟随、*等。)有助于找到所有的依赖项，并记住什么先做，什么后做，以避免开发人员相互阻碍。如果您发现某些票证应由某个团队优先处理以取消阻止其他团队，您可以根据这些链接选择票证优先级。](http://web.archive.org/web/20221001235740/https://www.netguru.com/blog/jira-tips-tricks-top-5-jira-extensions-for-effective-organization-and-optimization) 

![Screen Shot 2019-04-01 at 11.16.36](img/4d1295df012382439411060b0d18d0f0.png)

Sprint goal - this one is really important. It doesn’t matter that you delivered the tasks in one area or the other as planned if your team doesn’t have a common objective and a wider perspective. At Netguru, we try to [maximize business value](/web/20221001235740/https://www.netguru.com/about-us/press) delivered to our clients with each iteration. In order to maximise this process, we are always trying to state our objective for the next iteration. This should be specific and measurable. It helps the team to focus and set priorities when things go south and it helps the team to support each other in order to achieve this goal.

**Planning a release**
JIRA provides great tools to plan our deployments, which are a part of delivering what we accomplished with each of our iterations.
![Screen Shot 2019-04-01 at 11.16.47](img/465094ab68f1ad5955bd49d6addda2ef.png)

If the team is big, there is a good chance that you have many issues each sprint that need to be deployed and with that comes the risk that something can be lost by accident. Releases help us manage this by adding a *fix version* to each issue. Each release can contain a number of issues that we want to deploy. It also shows a progress bar so we can see how much work we still have left before all the issues in the release are ready to be deployed. Thanks to [our integration with Github](http://web.archive.org/web/20221001235740/https://www.netguru.com/blog/jira-tips-tricks-top-5-jira-extensions-for-effective-organization-and-optimization) we can also see the status of all pull requests for certain issue.
![Screen Shot 2019-04-01 at 11.16.57](img/751720f029d2c88d029dd8415aef2928.png)
The *Release* feature is a great tool for the team, but also for our clients. Our clients often use JIRA along with our team. This way they can always check the release notes, dates, and statuses of particular issues in order to see what has been deployed and what is still waiting to be deployed. Releases therefore help us be very effective with our reporting - the report is always there waiting for us if we need it!