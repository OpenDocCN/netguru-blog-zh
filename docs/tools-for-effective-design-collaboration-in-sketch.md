# 在草图中实现有效设计协作的工具

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/tools-for-effective-design-collaboration-in-sketch>

 在我的职业生涯中，我学到的最大的一个教训是，没有一个“完整的”、全面的、全栈的天才设计师。设计是一项协作运动，所以问题是:当多人远程处理相同的文件时，如何以一种智能和高效的方式进行协作？在 Netguru，作为一个 30 多人的团队，这是我们每天都要面对的挑战，经常是成对或小组远程工作。我们在 Sketch 中创建我们的设计，所以我们仔细研究了可以与 Sketch 集成的工具，以使我们的协作更加高效。

说到设计流程，这些年来已经有了显著的改进。市场最终意识到，那些庞大、笨重、功能齐全的工具是多么无效，它们是多么完全不适合快速响应的数字世界。许多伟大的工具可能已经出现，但是有一个问题直到去年还没有解决:版本控制。版本控制是一个简单的概念，现在的开发人员无法想象没有它的生活。作为设计师，我们不得不等待很长时间，直到有人意识到我们的努力，但我们最终得到了我们的版本控制系统，选择比以往任何时候都更广泛。

## 选项 1: Google Drive/Dropbox

![Tools for effective design collaboration in Sketch2](img/6a51ee8b4c9e033bb792b45754218776.png)

简单的库与 Google Drive 桌面应用同步设置

【2017 年 10 月，Sketch 发布了第 47 个版本的库。库本身并不意味着版本控制，但是它们允许用户在多个文档中共享符号。当与 Google Drive 或 Dropbox 一起使用时，库对于小团队来说非常方便——你可以将工作分成易于管理的部分，这些部分都基于相同的视觉组件。如果一些符号需要在此过程中更新，您只需编辑库文件，其他队友会收到更改通知，并能够同步该文件。

这一切都非常方便，设置也很容易(尽管记住，你需要一个[Google Drive](http://web.archive.org/web/20221202101102/https://www.google.com/drive/download/backup-and-sync/)/[Dropbox](http://web.archive.org/web/20221202101102/https://www.dropbox.com/)桌面应用程序才能工作)，但仍然有一些缺点。首先，文件版本冲突的问题依然存在。如果两个人同时试图编辑库文件，他们注定要失败。您必须手动合并库，并修复添加到设计文件中的任何新符号。随着参与一个项目的人数的增加，合并的问题可能会增加，所以这个解决方案可能更适合较小的设计团队。它不允许您同时处理相同的文件——您只能在它们之间共享符号——但是考虑到它是免费的，如果您不使用任何其他工具进行版本控制(或者没有预算),它仍然值得包含在您的工作流程中。

选项 2:摘要

## Sketch 发布库的前几个月，2017 年 7 月，又有一件事震动了设计界: [的公开推出摘要](http://web.archive.org/web/20221202101102/https://www.goabstract.com/) 。这个让每个人都超级兴奋，因为它相当于 Git，但对于设计师来说——几乎是我们所寻找的一切！问题是，对于以前有过编码经验的人来说，这个工具会感觉更直观，而分支、提交和合并的概念对于其他人来说可能会更难理解。然而，当你掌握了它的窍门，你就会体会到抽象能给你的工作带来的价值。摘要允许您查看文件的完整历史(突出显示特定的画板和符号)并查看单个提交中的所有单个更改(包括非可视更改，如所使用的草图版本！).

Abstract 提供了一个选项来标记您正在进行的更改(“工作进行中”、“准备好进行评审”等)。)并在 app 内部互相反馈。重要的是，你可以用 Slack 整合所有这些。如果你每天使用 Slack 和电子邮件来给出和接收反馈，这可能会派上用场。添加另一个反馈渠道可能会导致许多误解、遗漏，并因此降低效率，这首先违背了版本控制的目的。

什么是绝对的游戏改变者，是团队中的每个人都可以同时工作于一个功能，而其他人仍然可以预览和评论它，而不需要经常在多个草图文件/视觉板/jpg 之间切换。然后，您可以利用 Abstract 的冲突解决功能，而不是手动合并所有文件或复制粘贴画板或单个元素。每当您根据另一个团队成员所做的更改将您的更改与主分支合并时，您可以检查所有相互编辑的元素并挑选最终版本。虽然这听起来像玫瑰和彩虹，但仍有改进的空间。

分支的主要问题是，每当你想做出改变时，你必须创建一个新的分支。是的，这也包括一些小的改动，比如将一个按钮向上移动 10px，或者在某个地方更改副本。当你连续做了一些小的改动，还必须为你的队友提供评论时，这可能会有点令人不安。

当你补充说每个文件都必须从抽象窗口打开——而不是 Finder 或 Sketch——你可能会得出这样的结论:这可能会占用你太多的时间，而你本可以更有成效地使用这些时间(例如，进行实际的设计)。出于这个原因，我强烈建议只对库使用它，然后在独立文件上处理特定的视图。

被跟踪文件的历史摘要

![Tools for effective design collaboration in Sketch3](img/5e7e051a89e4437dbc456e18aa111c29.png)

工厂中被跟踪的文件历史

选项 3:工厂

![Tools for effective design collaboration in Sketch](img/c41fe2edead502daeb52ad377266c550.png)

虽然在设计环境中实现典型的开发人员一对一工作流程的概念初看起来很吸引人，但是有必要思考一下这些流程之间的区别。同样的原则适用于他们两个吗？根据 [Plant、](http://web.archive.org/web/20221202101102/https://plantapp.io/) 的创作者所说，你无法真正将开发者的经验直接转化到设计过程中。Plant 虽然仍然源自经典的版本控制系统，但它不包含分支，并且只支持线性历史。这确实提升了整个过程，所以如果 Abstract 的高级特性让你头疼，Plant 可能是你最好的选择。Plant 本质上是一个草图插件，在开始工作之前，你不需要打开其他任何东西。完成后，您只需从侧面板中选择一个选项，就可以开始了(您也可以使用键盘快捷键)。合并冲突看起来类似于在抽象中完成的方式，除了您在 Sketch 中通过与画板交互来完成。Plant 似乎更符合设计工作流程，更直观。

然而，要注意的是，植物的简单也带来了一些缺点。缺乏更高级的审核流程会导致这样的情况，在这种情况下，你只能绝对地处理问题——要么接受改变，要么拒绝，不做进一步的评论。此外，插件只允许每个项目一个文件(不像 Abstract，你可以创建整个集合)，这可能会给一些人带来不便。

## 总结

就同步设计师的协作努力而言，去年是一个改变游戏规则的一年。虽然目前可用的工具肯定是朝着正确的方向发展，但我们还没有看到最终的解决方案。很难预测它是更像 Git 的版本控制还是 Figma 的实时协作。2018 年一定会带来一些新的东西——随着草图的 [野餐插件](http://web.archive.org/web/20221202101102/https://picnic.design/) 或 [Invision 的设计系统管理器](http://web.archive.org/web/20221202101102/https://www.invisionapp.com/blog/announcing-invision-design-system-manager/) 即将推出，没有其他事情可做，但请舒适地坐在我们的椅子上，见证这场小革命。

Beware, however, that Plant’s simplicity also entails some shortcomings. The lack of more advanced review process leads to situations, where you only deal in absolutes – you either accept the change or dismiss with no further comments. Also, the plugin only allows one file per project (unlike Abstract, where you can create whole collections), which might be an inconvenience for some people.

## Summing up

Last year was a game-changer when it comes to synchronizing designers’ collaborative efforts. And although currently available tools are certainly heading in the right direction, we’re yet to see the ultimate solution. It’s hard to predict whether it is something more of Git-like version control or Figma’s real-time collaboration. The year 2018 is bound to bring something new to the table – with the [Picnic plugin](http://web.archive.org/web/20221202101102/https://picnic.design/) for Sketch or [Invision’s Design System Manager](http://web.archive.org/web/20221202101102/https://www.invisionapp.com/blog/announcing-invision-design-system-manager/) launching soon, there’s nothing else left to do, but sit comfortably in our chairs and witness this small revolution.