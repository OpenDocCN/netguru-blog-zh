# 我们如何在 9 个月内将项目可预测性提高了 102%。你也可以修复你的公司。2

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-we-improved-project-predictability-by-102-in-9-months-part-2>

 在我们寻求更多的可预测性的过程中，一些有着领先软件公司工作经验的顾问拜访了我们。他们应该分享他们的工具和流程，并帮助我们找出我们可以改进的地方。事实上，他们帮不了我们。我们的可预测性高于他们，他们认为没有必要进一步提高。

当我们无法从别人的错误中吸取教训时，我们会求助于硬数据。在本系列的第 1 部分中，我们谈到了在项目结束后的 AAR 上测量我们的可预测性。过去是 40%——我们只能以 40%的准确率预测项目的进展。

那是不可接受的。所以我们进入改变模式，把它提高到 81%左右。

这是我们关于在[软件开发和咨询公司](/web/20221209115002/https://www.netguru.com/services/software-development)提高可预测性的三篇文章系列的第二部分。查看[第 1 部分](/web/20221209115002/https://www.netguru.com/blog/how-we-improved-project-predictability-by-102-in-9-months-part-1)，了解背景信息，并获得向同事介绍重大变革的建议。[第 3 部分](/web/20221209115002/https://www.netguru.com/blog/how-we-improved-project-predictability-by-102-in-9-months-part-3)为我们提供了提高可预测性的分步指南，以及您可能想要采取的下一步措施。

![5](img/35c432df46a0cd6fed29a3c384766807.png)

1。评估-我们如何利用 13 世纪的数学来改善我们的业务

## 评估对短跑的成功有直接影响。直到去年，我们一直使用一种混合模型进行评估——有点像基于点的系统和基于时间的系统。我们把小时(即时间)换算成分， [是因为人们不能正确估计小时](http://web.archive.org/web/20221209115002/https://www.agileconnection.com/article/user-story-points-versus-man-hours-estimating-effort-better?page=0%2C0) 。

随着时间的推移，我们切换到了基于 [斐波那契数列](http://web.archive.org/web/20221209115002/https://en.wikipedia.org/wiki/Fibonacci_number) (是的，13 世纪的智慧在现代技术中实现)的唯分制。基本思想是，你使用参考任务，并通过与它们的比较来分配分数——不是在 1 到 10 的范围内，而是在从一个值更快地前进到另一个值的范围内(斐波那契数列的修改后的开始如下:1，2，3，5，8，13…)。分数是根据任务的复杂程度而不是持续时间来分配的。这种方法源于我们大脑的工作方式。

2。内聚方法学

## 我们一直以敏捷的方式管理项目，但起初，我们凭直觉行事，而不是依赖确凿的事实。我们的方法不够系统——我们会根据特定的客户和情况改变我们的方式。在 2017 年的前两个季度，我们决定巩固我们管理项目的方式。这并不是说我们突然变得超级严格，而是我们现在使用固定的元素(站立、冲刺、回顾和演示),围绕这些元素我们建立每个项目的生命周期。

我们一直希望我们的方法是一致的。我们觉得我们必须继续像以前那样做事(比如乱糟糟的)，或者完全投入到 Scrum 中。幸运的是，我们已经接受了我们需要调整自己以适应市场现实，本着敏捷的精神，并采用适合我们需求的方法。老实说，你需要成为 100% Scrum 的规则与敏捷思维背道而驰， [越来越多的人以我们](http://web.archive.org/web/20221209115002/https://www.youtube.com/watch?v=vSnCeJEka_s) 的方式处理这个问题。

![NG_visuals_dark-04](img/aea2a79745d8a62dcf8d65b0f25ab5ba.png)

3。我们的生态系统——吉拉是项目的核心，Salesforce 是业务的核心

如果不考虑你使用的工具，就不可能开始研究可预测性。基本架构如下: **数据点+ Salesforce + Slack** 日常沟通。最重要的数据进入 Slack，这样我们可以以一种可访问的格式实时看到发生了什么。

![7](img/7126a24fa6908e0c2923baebf339ddb8.png)

我们还利用吉拉来组织我们的每个项目。它有助于我们理清思路，清楚地展示项目最终结果的期望。它让我们能够轻松地根据不断变化的环境调整项目计划。最重要的是，吉拉有很多分析工具，让我们衡量我们在一个给定的项目中做得如何。

## 理想的状态是高效的**使用吉拉，[销售力量](/web/20221209115002/https://www.netguru.com/services/salesforce-development)和懈怠。**

 **一旦我们开始处理数据，我们注意到我们的工具是多么的不整合。我们用一种工具安排时间，用另一种工具处理客户关系。他们没有很好地融合，他们不能给我们创建指标和做出好的决策所需的数据。因此，我们将调度转移到 **Salesforce** 并整合了其他数据点(吉拉、Github)。在 Salesforce 中以正确的上下文(业务和个人项目)显示数据改善了我们的流程，帮助我们组织工作，并对我们的可预测性产生了非常积极的影响。

我们仍在努力简化生态系统，尽管我们已经在 2016 年改进了大部分想要改进的地方。我们有两个相互联系的信息来源。吉拉是这个项目的核心，而 Salesforce 是我们业务的核心。吉拉帮助我们制定计划，而 Salesforce 让我们从整个公司的角度来审视每个项目。连接这些工具使我们能够通过 KPI(关键绩效指标)评估 Salesforce 内部的项目运行状况，而无需分析吉拉中的所有元素。

尽管如此，一些小事情仍然需要简化。我们有太多的“卫星”工具，我们正试图简化工具架构。我们很有可能会继续努力改进。我们现在正朝着机器学习前进，以便能够做出更快的预测。

![3](img/4a15f75ca2e14b5399372244040f5622.png)

数据不仅在项目管理中很重要。我们尝试连接来自不同部门的数据，Salesforce 帮助我们做到这一点。

4。处理数据——事实的单一来源

过去几年，我们一直在使用 Salesforce。最初，我们把它当作一个标准的 CRM，只利用 CRM 的功能。随着时间的推移，我们开始发现 Salesforce 的全部功能，并将其用作我们的单一真理来源(SSOT)，特别是因为它还允许构建多个版本的真理(MVOT) ( [这里有更多关于这些概念的信息](http://web.archive.org/web/20221209115002/https://hbr.org/2017/05/whats-your-data-strategy) )。

第一步是将有关我们项目的信息转移到 Salesforce 中，这样它就可以成为我们的“项目 CRM”。第二个重要的步骤是将我们的时间安排(例如，项目的人员分配)也移到那里。我们研究了现有的专业服务自动化解决方案，但最终，我们决定自己创建一个。我们认为其他解决方案还不够成熟，无法满足我们的需求。回过头来看，我们认为这是一个正确的决定。

完成后，我们决定在 Salesforce 中围绕我们的项目收集更多数据。我们使用来自 JIRA 和 GitHub(我们的代码库)的数据，创建了一个自动计算项目 KPI 的工具。它收集计算 KPI 所需的数据点，并将它们转换为项目度量，这使我们能够在早期识别项目中的潜在问题。

## 可预测性的关键:指标

现在，可预测性只是我们衡量的众多 KPI 之一。我们必须创建度量标准并学习如何使用它们，但是这种努力是值得的。基本步骤是:

教育我们自己

创建指标

## 使用我们的新指标

我们开始寻找能够帮助我们衡量一个给定项目进展如何的指标。我们首先与整个项目团队开了几次会。我们将项目分成几个部分，并检查我们是否能在这些部分中看到问题。这不是一件简单的事情，因为我们手头总是有相当多的并行项目。

2.  我们想出了大约 10 个 KPI，现在我们为每个项目衡量这些 KPI。
3.  两个基于全程短跑测量:

sprint 实现的百分比(sprint 计划的工作在结束前完成了多少)；

在 sprint 开始和初始规划阶段后添加到 sprint 中的票证。

![mps-logo-grayscaleZasób 2full](img/3845fd520b15929c5be3b8fb6cec1039.png)

其余的我们每周测量，因为 sprint 的长度在我们所有的项目中并不相等:

交付的积分(门票的积分值)；

票据等待代码审查的平均时间(营业时间)；

*   票据等待质量分析的平均时间(营业时间)；
*   通过质量分析被拒绝的票的数量；

报告的 bug 数量；

*   每个开发者的平均提交次数；
*   提交次数最少的人(开发人员)提交的次数。

*   使用指标
*   我们每周都会查看项目指标，根据这些数据，我们判断项目的健康程度。我们衡量我们在一个给定的 sprint 中取得了多少成果，并与开始时的计划进行比较。这直接转化为可预测性。
*   跟踪流程效率

这是 2017 年 9 月我们完成冲刺的百分比。这是一个相当令人振奋的结果。但这不是唯一要考虑的因素。我们引入了许多流程，帮助我们收集更详尽的数据，并几乎实时地跟踪我们的结果。

![8](img/be0ebf04630e7efe305db10575b9bd58.png)

**项目-外部 PM 项目评审**

## 一个常规的过程，在这个过程中，项目外部的 PM 关注项目是如何进行的，主要关注来自工具的数据。

**敏捷评审**

## 几乎是一样的东西，除了它是一个更深层次的观察，并且专注于方法论。团队必须积极参与。

**项目关键绩效指标**

![NG_visuals_dark-03](img/03a72fc9be4c5edef9cc1349777cfe34.png)

他们每周给我们一个项目的快照，帮助我们了解项目可能面临的挑战。

**回顾会**

团队分析最后一次冲刺并做出改进。

**反馈会议**

我们为项目经理和客户成功管理人员举行反馈会议。理想情况下，我们也有客户在那里，了解他们的观点。

当我们开始进行工具集的统一和评估时，我们只有一些衡量标准:客户停留多久，他们为什么离开，他们给出了什么样的反馈。这并没有让我们看到全貌，所以我们开始寻找硬数据。现在，我们收集数据，帮助我们在几个层面上衡量流程的有效性。

**Project KPIs**

They give us a snapshot of the project every week and help us see what challenges the project might face.

第二部分结论

在这个由三部分组成的文章系列的第二部分中，我们讨论了如何利用我们已经收集的数据，并介绍了适当的指标和流程来衡量和提高我们的可预测性。[第一部分](/web/20221209115002/https://www.netguru.com/blog/how-we-improved-project-predictability-by-102-in-9-months-part-1)讲述了正确预测你的业务流程有多重要，以及给你的公司引入重大变革的困难。[第 3 部分](/web/20221209115002/https://www.netguru.com/blog/how-we-improved-project-predictability-by-102-in-9-months-part-3)是我们提高贵公司可预测性的指南。

**Feedback sessions**

We hold feedback sessions for PMs and customer success executives. Ideally, we have the client there as well to learn about their perspective.

When we started with toolset unifications and estimation, we had only a few measures: how long clients stay, why they leave, what feedback they give. That didn’t show us the full picture, so we started searching for hard data. Now, we gather data that helps us measure the effectiveness of our processes on several levels.

![9](img/b743dc0b90ae3396e80a12d9064e825c.png)

## Part 2 Conclusions

In this second episode of our three-part article series, we talked about how we took advantage of the data we already gathered, and introduced proper metrics and processes to measure and improve our predictability. [Part 1](/web/20221209115002/https://www.netguru.com/blog/how-we-improved-project-predictability-by-102-in-9-months-part-1) deals with how important it is to properly predict your business processes and with the difficulties of introducing a major change to your company. [Part 3](/web/20221209115002/https://www.netguru.com/blog/how-we-improved-project-predictability-by-102-in-9-months-part-3) is our guide for improving predictability in your company.**