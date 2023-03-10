# 好习惯如何提高我的测试自动化技能

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-good-habits-improved-my-test-automation-skills>

 我不得不承认，在我在 Netguru 的将近一年半的时间里，我的自动化测试技能并不总是一致的。我努力使我的测试模块化，易于编写和维护。尽管如此，学习一些好的习惯、页面对象模式和减少多任务处理的数量大大减少了我为了编写可管理的和干净的测试所付出的努力。

有各种各样的原因可能会阻止你在[测试自动化](/web/20221007084610/https://www.netguru.com/services/test-automation)中养成良好的习惯。短期项目、复杂的前端代码、涉及一些实验的移动项目，或者缺少合适的工具来做这些。当你不够了解时，不连续性也是很常见的。然而，改变你的习惯将对提高你的测试质量产生重大影响。如果你想学习好的实践，一定要坚持阅读。

### 保持你的测试干燥

一条古老的好建议:不要重复你自己。听起来很简单，对吧？事实上，这很简单，但是作为一个新手，你可能会感觉到创建更长文件的内在需要，因为你产生的代码越多，你取得的成就就越多，对吗？不对。为什么会这样呢？它类似于极简主义设计。首先，你要么尽可能描述性地设计或编写测试。然后，使用助手、页面对象和其他智能方法来减少实际规格中的代码量。有什么帮助？编写的代码越少，通常意味着工作量越少。

![3034240-poster-picasso-011735-edited.jpg](img/029432a0b95551a2e2d03cb1e3ba466d.png)

我以前的作品是什么样的:

![2-search_engine_spec.rb+E28094+7E2Fprojects2Fhungr.api+2017-02-27+08-16-36.png](img/1766e9a2c8b5a383423b8d58ff9255cb.png)

今天的天气:

![test.rb — --projects 2017-03-17 14-56-02.png](img/48a6f4928eac3a1cb73086f2e92463ad.png)

哦，我的天，所有的线都到哪里去了？只有理解了简化的过程和底层结构是如何工作的，你才能最大限度地减少代码量。因此，我不会解释案例#1 和案例#2 之间发生了什么。请注意，即使是第二种情况，仍然可以稍微改进，但是我相信您已经注意到了。

### 将你的测试分成几个部分

无论你是在编写你的第一个测试，还是你已经有了这个领域的经验，计划/构建都很重要！没有办法不先计划好就直接开始编写测试。你到底怎么做到的？

既然你正在读这篇文章，我敢肯定你已经听说过 SitePrism，一个水豚的页面对象模型 DSL。然而，如果你没有，请[到这里](http://web.archive.org/web/20221007084610/https://github.com/natritmeyer/site_prism)。这将极大地减少你花在编写测试上的时间。SitePrism 允许你将 web 应用分成页面，将页面分成部分。

在设计测试的时候，我养成了一个习惯，那就是画截图并把它们分成部分和元素，这给了我将要使用的元素的可视化表示，例如:

![Pasted image at 2017_03_17 02_04 PM-103003-edited.png](img/aa55e06e0874d21ba1c959139f200c28.png)

您会发现这种做法非常有用，因为它简化了您正在处理的页面。我称之为逆向原型。有了这些，下一步就是打开 atom/sublime 或者你选择使用的任何编辑器。作为一个整体绘制的每个截图代表一个单独的页面，对于每个页面，您需要创建一个名为 your_awesome_page.rb 的文件。页面包括节(your_awesome_section.rb)，节包括模态(your_awesome_modal.rb)，您甚至可以进一步划分模态，但是，嗯，我不认为这有任何用处。

找到模式

![4-divide-by-zero6-991718-edited.jpg](img/473f32eccdfc8331a71537a1411e5a6c.png)

你可能已经注意到测试是关于重复的。在您参与的项目之间切换时，很可能会发现您的测试中有相当大的一部分几乎是相同的。因此，为什么不用更早的测试作为参考点呢？在计划测试时，您通常可以考虑两个部分:

### 你可以从你的旧项目中“引用”的测试，

你必须尝试的新测试。

*   如果你喜欢参考你的旧测试，确保你没有从你的电脑上删除它们。Github 库可以被删除，没有人会在乎你是否把你的测试宝贝放在那里。
*   一些可以在项目之间重复的事情:

登录，

注册，

*   添加用户，
*   活动管理员，
*   还有很多很多。
*   尽管如此，当提到你以前的工作时，有一点必须考虑进去。回头看看你以前做过的事情，你总是可以改进的，所以不要在没有反思什么可以做得更好的情况下去做。
*   从不一心多用

有些人说，他们能完成工作或更有效率地工作的唯一方法是多任务处理。我确信你们所有人都知道至少有这样一个人，他不停地说“看我有多棒，看电视连续剧或和你聊天时我是多么专注于编程。”公牛***，他们就是错了。我自己不是心理学家，也不是生产力专家，但从我自己的经历来看，我可以告诉你，在一家[软件开发公司](/web/20221007084610/https://www.netguru.com/services/software-development)工作，多任务处理是最大的痛苦。为什么？想象以下情况:

### 您正在开发项目 X，测试一个新的复杂特性，该特性要求您同时使用多个用户登录。出乎意料的是，您收到了来自项目 Y 的项目经理的一条松散的消息:“嘿，您能帮我检查一件事吗？真的很急，当事人都气疯了”。

起初，当我还是一个没有经验的质量保证工程师时，我会心甘情愿地去做，做任何要求我做的事情。但是回到以前的任务往往会很痛苦。我真的不知道我在哪里停下来了，我不得不从头再来。或者我不得不猜测我停在哪里，从估计的点继续前进。

一心多用本身就像读前一句话一样可怕。相反，你应该更喜欢这样:“是的，我会帮你交配，但在 20 分钟内，因为我有一件事要完成”。就是这样。你再也不用一心多用了。我认为一心多用是有害的，这个领域一些比较可信的人也是这样认为的。关于这个主题的更多信息，请参考本文。

总而言之，当编写自动化测试时，从准备一个要做什么的简明计划开始，然后将它分成更小的任务，这是一件好事。在您开始测试工作之前，在您的日历中找到一个相关的窗口来完成工作，并将您的松弛状态设置为“忙碌”。就个人而言，我喜欢在成功部署后编写测试，因为这样我就知道我有一些时间可以卓有成效地“花在”持续的测试开发上。

Multitasking itself is as dreadful as reading the previous sentence. Instead, you should prefer to roll the following way: “Yes I will help you mate, but in 20 minutes because I have a thing to finish”. That’s it. You will never have to multi-task again. I think multitasking is harmful, and so do some more credible people in the field. For more on that subject refer to [this article](http://web.archive.org/web/20221007084610/https://www.forbes.com/sites/work-in-progress/2013/01/15/how-multitasking-hurts-your-brain-and-your-effectiveness-at-work/#57e433fc1013).

All in all, when writing automated tests, it is a good thing to start with preparing a concise plan of what to do and then divide it into smaller tasks. Before you start working on your tests, find a relevant window in your calendar to complete the work and set your Slack status to `busy`. Personally, I like writing tests after successful deployments because then I know that I have some time which I can fruitfully `spend on` continuous test development.