# 我们如何跟踪在 Netguru 工作的 200 多人的技能

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/track-skills-200-people-working-netguru>

 嗨伙计们！

我叫 Darek，是 Netguru 的一名领导者和高级 RoR 开发者。我加入 Netguru 时，它只有 30 名成员，现在我们已经发展到 210 多名了！最近，我有一个非常有趣的问题要解决。我们希望更好地了解我们的员工拥有哪些技能，他们希望在哪些方面做得更好，最后但同样重要的是，改进我们为项目分配人员的方式。这就是为什么我们引入了“技能”，这是我们的开源项目之一“人”中的一个特性。如果你愿意，你可以自己尝试一下。让我向您介绍一下新功能！ 

## 我们做什么，为什么做，如何做

前段时间，我们创建了一个名为“People”的开源项目，它帮助我们跟踪谁在做什么。因为我们的公司发展迅速，我们参与的项目数量也在增加，我们需要更好地评估和跟踪开发人员的技能，以便能够根据他们的知识和经验以及他们的兴趣将他们分配到项目中。在与领导、高级开发人员和人才经理讨论这个问题后，我们决定为“人”添加新功能。我们在 Netguru 增加了许多设施来跟踪知识和人们的职业发展。现在起作用了，太牛逼了！多亏了这个解决方案，我们 100%确定我们为客户的需求分配了合适的人才。看看下面的新功能。

## 新功能

我们从不同用户的角度对这些功能进行了细分。您可以探索不同模块的功能:

*   开发者
*   领导者
*   人才经理

## 开发者视角

这是开发者主页的样子:

![image05-2.png](img/e8a6c0936782bb19c5c1ea3d08913513.png)

该页面允许用户查看按类别分组的技能，对他们的技能进行评级，标记为最喜欢的，并添加注释。

现在，开发人员可以在不同的部门清楚地看到哪些技能对公司很重要。他们可以告诉其他人他们想在哪些方面做得更好，留下笔记等等。跟踪进度和给项目分配人员要容易得多。

评级

### 我们改进了技能评级机制。现在，我们有两种评级:

第一种类型适用于较小的技能，如 CoffeeScript。这里的评分是二元的:

![image02-4.png](img/310c4e8ae8f3dcc5d596367c3642c6f1.png)

**0**——我不知道工具/方法论/语言/模式。

**1**——我知道工具/方法论/语言/模式。

另一类是针对更高精尖的技能，比如 React.js 中的熟练度，这次的尺度是从 0 到 3。

T2——我从来没用过。我对这个工具/方法/语言/模式没有任何经验。我没有足够的信心把它用在一个项目上。

T2——我以前用过一次。我知道一点儿。我没有足够的信心把它用在一个项目上。我需要重新阅读文档。

T2——我已经用过几次了。我觉得我的知识足以在项目中使用它(偶尔使用文档)。我理解这个概念。

**3**——我已经使用这个工具/方法/语言/模式很多次了。我很自信我可以把它用在一个项目上。我很少需要检查文档中的某些东西，如果有的话。

最喜欢的技能

我们增加了将一项技能标记为最喜欢的选项，这意味着将某项技能标记为最喜欢的人希望在这方面做得更好。

![image12-1.png](img/abbaa96bd8b5bb6756a0f04bca16fa47.png)

留言

有时，你会想添加更多关于你在某项技能上所做的事情的背景信息。

例如:我在做后台工作，但主要是和 Sidekiq、DelayedJob 和 Cron 一起工作。

按类别划分的技能

![image03-3.png](img/7121bfbf8686eac0acf13b0448a572a2.png)

在 Netguru，我们将代表不同部门的技能进行了分类。我们想弄清楚我们对不同部门成员的期望。最重要的是，现在每个人都可以检查他们还能学到什么。当然，这并不意味着他们应该总是坚持自己的主要类别，绝不是说，他们可以——甚至应该——浏览其他类别，以找到发展新技能的灵感，并让其他人知道他们知道哪些更酷的东西。

### 领导观点

Netguru 的领导注重人员教育，提高他们的技能和知识，等等。每个季度，每位员工都会召开一次面对面的会议，与领导讨论他们取得的进步。现在，领导者能够检查在选定的时间范围内，他们团队中选定成员的哪些技能发生了变化。领导主页如下:

![image10-1.png](img/5573d13e38a7bca6270087ee745c6371.png)

技能等级历史

技能变化也是按类别分组的。您可以选择希望技能评分如何变化的时间段。图表只显示已经改变的东西。如果在选定的时间段内没有发生变化，图表将为空。

## 特定技能等级

领导只能看到团队成员的技能水平。另一方面，人才经理可以访问公司中所有人的技能评级。

![image04-2.png](img/42245020e3e188df64a1c491d7c2892a.png)

### 人才经理视图

人才经理可以通过技能搜索用户。人才经理的主页看起来和领导者的一样(看看 特定技能等级 ):

### 按技能选择的用户:React Native、Ruby 和牵线木偶. js

我们添加的其他酷东西

**![image07-1.png](img/3008f0853e23b0d2edb89238eb46c735.png)**

## 改进了创造新技能和编辑现有技能的系统

我们希望防止任何人都可以添加和编辑技能的情况。我们引入了一种创造新技能和编辑现有技能的新方法。管理员、领导和人才经理可以创建请求，以创建新技能或编辑现有技能。他们需要留下一个简短的解释，为什么他们想改变或增加一项技能。另一名行政人员、领导或人才经理将审查该请求，并决定是否应接受或拒绝该变更。当然，他们也需要解释为什么做出这个决定。

![image08-1.png](img/7020bb9deaa6c259d872503af146cd05.png)

## 编辑现有技能

### 审阅变更请求

我们在单独的页面上跟踪技能描述、评分和类别的所有变化。

你可能想知道我们是否会将变化通知其他人。答案是肯定的——我们已经创建了一个系统来通知人们相关的变化。有两种不同类型的通知:

![image13-1.png](img/508f59916ed2067d3ccf4c0d3ca74076.png)

创建新技能时:我们会向所有用户发送通知。

当一个现有技能被编辑时:我们会向所有受影响的用户发送通知。

![image01-6.png](img/a7ff3b6424ad1396d0c05247e0beece7.png)![image11-1.png](img/f997a119a5fe05f989bdb491d4e5858d.png)

新技能已经创建的通知

当您点击通知时，您将被重定向到一个页面，在该页面上您可以看到更改并更新您的评分。

*   改进的用户资料:展示技能
*   你想看一个人的所有技能吗？没问题，看看他们的简介，在技能部分。在那里，你会找到所有的技能和评级，以及他们想要发展哪些技能的信息。

API 集成

![image06-1.png](img/0be39d97171fdacec16b9997d1f09269.png)

我们增加了一个新功能，但你将看不到它。每当您需要在不同的应用程序中使用技能等级信息时，您可以从现成的 API 端点获取数据。只需通过电子邮件指定您想要谁的技能，或者将范围设置为所有。

总的来说，我们为新变化带来的成就感到自豪。现在，“人”不仅使我们能够将所有的调度数据保存在一个地方，而且确保我们的开发人员做他们真正想做的事情，这转化为对他们日常任务的更多承诺。

When you click on the notification, you will be redirected to a page where you will be able to see the changes and update your ratings.

![image00-8.png](img/ecc54ae3262f139ecdf3d1e15ba9ac4d.png)

### Improved user profiles: displaying skills

Do you want to see all skills of a person? No problem, have a look at their profile, at the skills section. There, you will find all skills and ratings, and the information which skills they want to develop.

![image09-1.png](img/45d6d3a79e658731301b1f32afe7360a.png)

### API integration

We’ve added one more feature, but you will not be able to see it. Whenever you need to use the skill rating information in a different application, you can fetch data from a ready-to-use API endpoint. Just specify whose skills you want to have by email or set the scope to all.

All in all, we’re proud of what we have achieved with the new changes. Now, “People” not only enables us to keep all the scheduling data in one place but also ensures that our developers do what they really want to do, which translates into even more commitment to their daily tasks.