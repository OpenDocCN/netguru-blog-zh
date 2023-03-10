# 如何使用 Python 创建并实现一个 Bot？

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/creating-a-bot-using-python>

 谁不想更专注于解决创造性的问题，而不是在工作中执行重复性的任务？是的，有一个创新的方法来解决这个问题——创建一个简单的机器人。让我们一步一步地介绍如何做到这一点。

## 什么是聊天机器人，它能给我们带来什么好处？

根据定义，聊天机器人是一种计算机程序和/或人工智能，它通过文本方法进行对话，并根据上下文采取适当的行动。想象一下，你想预订一张确切日期的去某地的机票，并且你知道你想住在哪里(或者想列出威尼斯最便宜的 5 家五星级酒店)。你可以去航空公司的网页，找到航班，选择日期，购买机票，然后谷歌酒店和阅读他们的评论(每个在不同的网站)，检查他们的页面上的一些照片…15 分钟的工作会花你 2-3 个小时，你仍然需要收拾你的行李！或者，你可以问你的私人助理:“嘿，机器人鲍勃，你能帮我订一张 5 月 10 日至 15 日飞往威尼斯的机票，并告诉我最便宜的五星级酒店吗？”然后你会得到酒店的描述和照片——你所需要做的就是给出你的信用卡信息。这不是未来。这种机器人就在这里，事实上它们很容易编写。

[Python 作为编程语言，无论是初学者还是专业人士，都是首选](/web/20221002005956/https://www.netguru.com/glossary/python)。它易于使用，易于学习，其庞大的社区提供了大量现成的库和框架。它有一些花哨的框架，如 [TextBlob](http://web.archive.org/web/20221002005956/https://textblob.readthedocs.io/en/dev/) 或 [spaCy](http://web.archive.org/web/20221002005956/https://spacy.io/) ，帮助你用一些自然语言处理技能创建真正高级的人工智能。您可以使用 [ChatterBot](http://web.archive.org/web/20221002005956/https://chatterbot.readthedocs.io/en/stable/) 框架，它允许您在几分钟内创建一个基于机器学习的对话机器人！另一方面，如果你只是需要一个使用 Python 的简单工具，它将花费你大约 2 个小时(包括制作咖啡)，你不需要成为专家或开发者(仍然需要基本的编程技能)。

每个人都很忙，包括开发人员。他们也讨厌无聊、重复的任务，比如站立！每天早上他们都要回答同样的问题。日常聚会经常被认为是程序员的主要干扰。为了简单起见，我们可以让他们不用离开键盘就可以给出反馈——只需要几秒钟就可以在 Slack 客户端上写下个人信息。

见见站立机器人鲍勃！

让我们的生活变得更简单，创造一个机器人，用最简单的方式回答和记录问题。

在 Netguru，我们在日常交流中使用并热爱 Slack。它是免费的，非常直观，并且有一些很好的特性，比如用于第三方集成的 API。

## **准备，稳住，巨蟒！一步一步创建机器人**

首先，我们从创造工作环境开始。我们将使用 virtualenv 和 virtualenvwrapper，因为它们是帮助我们保持依赖关系整洁和可维护的工具。请查看您选择的操作系统的安装指南(virtualenv 和 [virtualenvwrapper](http://web.archive.org/web/20221002005956/https://virtualenvwrapper.readthedocs.io/en/latest/install.html) )。你也可以使用内置了 virtualenv 的 [PyCharm IDE](http://web.archive.org/web/20221002005956/https://www.jetbrains.com/pycharm/) 。

现在，我们可以在 Slack 名称空间中注册我们的 bot，这样应用程序将知道将所有通知发送到哪里。为此，请转到[https://api.slack.com/apps?new_app=1](http://web.archive.org/web/20221002005956/https://api.slack.com/apps?new_app=1)，填写应用程序名称并选择一个工作区。

![Screenshot 2019-03-22 at 15.12.53](img/fc3a61b68034a4c736c0ce1b48821d00.png)

然后转到新创建的机器人的**功能**部分下的**机器人用户**页面，创建一个新的。这将创建一个类似于松弛用户的对象，该对象稍后将被列在您的松弛用户列表中。

![Screenshot 2019-03-22 at 15.15.14](img/c5bf56bbd8db78e0a9ee1df1defe295e.png)

最后但同样重要的是，在安装应用程序页面点击安装应用程序。这将要求您授权 bot 访问您的工作区。因此，您将收到一个 **OAuth 访问令牌**和一个 **Bot 用户 OAuth 访问令牌**。这些非常重要，因为它们允许您的 Python 代码与 Slack 通信。

![Screenshot 2019-03-22 at 15.18.06](img/5d3479148c714e92eabb590b3c1cf51c.png)

## **给我一些代码！**

是时候写一些 Python 代码了。正如我们之前看到的，Slack 提供了一些漂亮的文档，他们有一个专门的章节用于 [Python 集成](http://web.archive.org/web/20221002005956/https://slackapi.github.io/python-slackclient/)。

Slack 使用发布者-订阅者架构。为了接收通知，我们需要订阅侦听器队列。为此，我们用 OAuth 令牌初始化 Slack 客户端，并通知它我们已经准备好运行[https://pastebin.com/qb3tVwbT](http://web.archive.org/web/20221002005956/https://pastebin.com/qb3tVwbT)。现在，您可以调试您的代码，并查看 rtm_read()方法返回哪种数据(以及返回的频率)。

现在我们应该开始我们的第一个命令——机器人对我们信息的实际反应。我们将使用 Python 中称为[鸭子打字](http://web.archive.org/web/20221002005956/https://en.wikipedia.org/wiki/Duck_typing)的东西来实现我们所有命令类的基础。如果它能“做”它，那么它就是一个可行的命令。我们可以使用一个简单的帮助命令。每当有人给我们的机器人写 *help* 的时候，它就会用它能识别的所有命令来响应。[https://pastebin.com/eXey19aA](http://web.archive.org/web/20221002005956/https://pastebin.com/eXey19aA)。

每个单口相声都以同样的问题开始:你在做什么？你遇到什么问题了吗？以后有什么打算？所以这是我们的用户表单，答案必须被记录下来。此外，站立必须有人开始，用户列表不能固定(有不同的项目组，有时有些人可能会失踪)。有些人在遇到问题时会想“我知道，我会用正则表达式。“现在他们有两个问题。” [杰米·扎温斯基](http://web.archive.org/web/20221002005956/https://en.wikipedia.org/wiki/Jamie_Zawinski)。我们需要使用正则表达式来匹配用户消息中的一些模式。如果消息以“起立:”开始，我们可以假设用户想要为一些用户开始会议。然后，我们需要找到所有的电子邮件，以确定我们需要向谁发送私人信息。【https://pastebin.com/msheRUbK】T4。

这将只是启动一个站，并向每个开发者发送第一条消息。但是我们需要把答案记录下来，最重要的是，发给原帖。为此，我们创建了一个 STANDUP_CHANNELS dict 集合来存储已经启动的 startups 线程。RespondStandupCommand 将检查传入的消息是否来自该通道，然后将它发送给管理器。[https://pastebin.com/CRi6cPqt](http://web.archive.org/web/20221002005956/https://pastebin.com/CRi6cPqt)

## **永不落幕的故事**

天空是每个编程项目的极限。您可以添加任意多的命令。您可以使用 ChatterBot 实现一个通信功能，并使用[语料库](http://web.archive.org/web/20221002005956/https://chatterbot-corpus.readthedocs.io/en/latest/)训练您的机器人。这是一个非常简单的 Slack bot 实现，但是它很有效，而且非常有帮助。我花了大约 2 个小时来解决一个现实生活中的问题，用户已经从中受益。接下来去哪里？我们可能应该使用线程库来使这个机器人不阻塞，更多细节参见[官方 Python 文档](http://web.archive.org/web/20221002005956/https://docs.python.org/3.7/library/threading.html)。此外，可以通过实现一个用于存储传入事件的[队列](http://web.archive.org/web/20221002005956/https://docs.python.org/3/library/queue.html)来提高性能。然后我们可以实现更多的工人，这样我们就可以支持更多的用户。[随意分叉&重用 Github](http://web.archive.org/web/20221002005956/https://github.com/PabloBuchu/bobthestandupbot) 上列出的全部代码。

在几行代码中，我们创建了一个工具，用于收集整个团队对其任务的反馈。除了让团队更容易专注于当前的工作而不是参加会议之外，它还有助于经理们保持密切联系，并对任何问题做出反应。你还可以创建许多其他的机器人:从讲一个随机笑话的机器人(或者根据需要给你发送一首随机的歌曲)，到帮助你在你的大楼里预定一个会议室的机器人——所有这些都只需要你的 Slack 客户机上的几个命令。