# 在你的应用中用 D3.js 实现数据可视化

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/d3-js-data-visualisation>

 当你创建一个产品时，你每天都要处理大量的数据——想想重要的信息，关于你的用户或者关于他们如何使用你的应用的细节。应该定期对其进行审查和解释——但是如何高效地进行呢？不管我们习惯什么，表格都不是显示数据的最用户友好的方式。大多数时候，这不是回顾具体数字，而是寻找趋势——这就是数据可视化发挥作用的时候。 

D3.js 是一个很棒的基于 javascript 的库，它让你有可能创建动态的、交互式的数据可视化，但它不是最容易使用的库。我们已经在几个项目中使用了它，我们想分享一些关于如何从中获得尽可能多的东西的技巧。为什么使用 D3，如何实现数据可视化来帮助你的用户？我们将向您介绍一些细节和一些关键优势。不多说了，我们开始吧！

### 我们应该如何可视化哪些数据？

数据可视化是以图形或图像格式呈现一组特定的数据。这样做的主要目的是让决策者看到数据本身以外的东西，观察趋势，掌握高要求的概念或确定新的模式。关键是让信息可见，但也要明智地选择可视化的内容。它可以是一个简单的衡量标准，也可以是三个不同的因素，它们之间没有明确的联系，但却相互依赖。根据您正在构建的产品，您可能会有由用户活动创建的不同类型的数据，这些数据不仅由浏览器收集，还由移动设备和环境中的其他传感器收集——如果您在物联网的世界中，这种可能性是无限的。

![Screen Shot 2017-07-18 at 17.55.51.png](img/fb47c029f9354bb7c44d85aa312ee83c.png)

*[样本数据可视化](http://web.archive.org/web/20221002003801/https://kiwanska.github.io/shiny-stuff/)——饼状图和条形图显示关于 netguru 开发者团队的详细信息(数据来自 2017 年 5 月)*

显示这种总结的最常见的地方是仪表板——我们希望为用户提供数据的多个视角以及查看单个数据点的能力。如果你的用户几乎每天都访问，那么可视化动态变化是很好的。当创建未来图表的概念时，你可以从草图开始，也可以从详细的需求开始。您可能想要利用多种交互可能性——例如，当单击某个特定元素时会给出更多细节，或者——如果您有两个对应的图表——突出显示每个图表中的哪些元素对应于另一个图表中的元素。数据可视化不需要看起来无聊或复杂才能发挥作用，也不需要复杂才能看起来漂亮。你可以从简单的条形图和饼图开始，或者创建一个复杂的(但要考虑周全！)图表。

### D3.js 的主要优势

D3(数据驱动文档)是一个 JavaScript 库，用于产生动态的、交互式的数据可视化。这是相当低的水平，开发者对最终结果有很大的控制权。D3 基本上有四个主要职责:

1.  从不同类型的文档中加载或导入数据，

2.  将数据绑定到特定的 DOM，

3.  借助 SVG 转换或控制所述元素的视觉面，

4.  响应于用户输入或活动而转变布局。

该库仅使用 HTML、SVG、CSS 和 JS 将数据带入生活，但当我们需要更多性能时，它也可以使用 canvas。

### 1.灵活性和可定制性

D3 没有任何预建的可视化——例如，没有现成的现成的柱状图方法，因为它的目标是低层次和灵活的。它可以是你想要的任何样子，它不会限制你的创造力！然而，有一个巨大的内置实用程序系统，用于创建和管理比例、颜色生成、弧线等。因此创建一个 **饼状图只需要几行代码** 。它不是一个图形库，也不是一个数据处理库——它是一个工具 **使数据和图形之间的连接更容易和更强大** 。和往常一样，这种自由也有一些缺点——D3 以其深度抽象和陡峭的学习曲线而闻名。然而，它是一个非常受欢迎和尊重的工具，有许多优秀的开发人员每天都在使用 D3。

![Screenshot 2017-07-14 08.59.28.png](img/aa6f643f0374bb5973cc95c12a43135c.png)

2.伟大的社区，伟大的灵感

### D3.js 的创建者 Mike Bostock 是《纽约时报》的前图形编辑，他擅长数据可视化、设计和开源。D3 背后的社区是巨大且受欢迎的——充满活力的开源社区意味着无论何时开发出现问题，答案都会在 StackOverflow 的某个地方找到！

此外，多亏了那些精力充沛的人，网络上有成千上万的图表示例[](http://web.archive.org/web/20221002003801/https://github.com/d3/d3/wiki/Gallery)**——其中大多数你可以找到公开共享的源代码。这给了设计者无穷的灵感，也给了开发者学习和改进技术方案的好机会。**

 **实际情况如何？

### 这些来自我们经验的具体例子将帮助你想象数据可视化带来的可能性和附加值。

1.追踪锻炼和饮食效果的详细折线图

### fitness app 专为健身链中的私人教练打造，能够记录和跟踪详细的身体测量、体脂和瘦体重等参数。显示进度的最佳方式是折线图——在这种情况下，我们使用带有轴和网格的详细图表，因此您可以看到每个数据点，并将其与日期相关联。客户与教练和应用程序合作的时间越长，我们拥有的数据就越多——因为我们希望展示图表的详细版本，这就是互动行为的帮助所在——整个图表可以放大或缩小，规则也相应地发生变化。在这样的项目中，所有这些元素(网格、轴、线)都可以作为单独的组件构建，因此它们只处理特定的一组数据，并负责整个图表的一小部分，这种结构可以重复使用，以实现更多类似的图表，但有一些小的差异。

2.通过关系和关联测量社会活动

![Screen Shot 2017-07-18 at 17.44.20.png](img/7279bd20c503d48a6a74adf08dc27f12.png)

每当我们处理特定的数字时，想象一下就很容易弄明白。如果你有的只是百分比或整数，而目标是显示它们之间的关系，那就困难多了。这时散点图会有所帮助——它是一种数学图表，使用笛卡尔坐标来显示一组数据中通常两个变量的值。例如，我们可以评估特定用户和他们的朋友之间的活动，并在散点图上显示他或她对特定人的回复速度，以及他们回复的速度和积极程度。数据可视化的很大一部分是强调数据之间的联系，而不是数字。

3.显示各种用户分析的复杂仪表板  

处理大量数据是一项挑战。GlobalWebIndex 是一款旨在为用户提供执行强大营销活动所需洞察力的产品，其主要功能是提供来自数字消费者研究的数据。为了显示这些统计数据，我们使用了:水平条形图、垂直条形图、饼图、圆环图、饼图雷达图、雷达图、径向条形图、折线图和发散条形图。为什么有这么多不同类型的图表？数据可视化就是选择最直观、最容易理解的方式来显示数据，同时也是为了避免视觉上的无聊。不要一个接一个地创建 5 个水平条形图，试着构建一个不同类型的图表的完整仪表板，呈现不同的数据方法，即使它们都共享相似类型的单位。

![Screenshot 2017-07-14 08.58.24.png](img/af8c0428191ec76299b888eebb218a99.png)

### 3\. Complex Dashboard Showing Varied User Analytics  

与基于 JS 的框架兼容

由于 D3 是一个基于 javascript 且与框架无关的库，它可以相对容易地跨各种不同的前端堆栈实现，无论你的应用是用什么框架编写的——React、Ember、Angular 还是 Vue。当使用 React 或 Vue 时，您可以创建很棒的可重用组件。对于基于 Ember 的应用程序，有很多插件和工具可以轻松实现 D3。如果你正在使用 Angular，你会发现使用 D3 和 typescript 有很多好处。好消息是:您也可以将 D3 与 React Native 一起使用！你所需要做的就是实现一个 react-native-svg 来呈现 svg 或者 [react-native 实现艺术库](http://web.archive.org/web/20221002003801/https://medium.com/the-react-native-log/animated-charts-in-react-native-using-d3-and-art-21cd9ccf6c58) 。

![Screenshot 2017-07-14 08.57.36.png](img/e7bacd1e43130cb22fc48ea78ce5005f.png)

### 如果你真的不需要所有的魔法

如上所述，D3 是一个相当苛刻的工具。如果你正在寻找一个更好的解决方案，看看这些工具: Chart.js，Highcharts，Recharts。它们非常适合创建简单漂亮的饼图或条形图，可以用在简单的数据上，并且可以非常快速地成功实现。预先设计好的图表是一种不费吹灰之力就能获得好的视觉效果的便捷方式(对 MVP 来说很棒)。当你的需求改变或者你的产品开发时会发生什么？如果你发现自己渴望在一个项目中使用两个不同的图表库，那么可能是时候给 D3 一个机会了。

总而言之

![Screenshot 2017-07-14 08.56.32.png](img/946a48ac92d0da59db58a6f4cedaf529.png)

D3 是创建定制的、美丽的、可视化的令人敬畏的方式。根据我们的经验，它是目前最好的数据可视化工具——有创意，得到广泛支持，并且易于在任何前端堆栈中实施。数据可视化为几乎每件产品增加了价值——不管它的主要特征是什么。我们希望上面分享的技巧能帮助你从中获得尽可能多的东西。

### If You Don’t Really Need All the Magic

As mentioned above, D3 is a pretty demanding tool. If you're looking for a more prepackaged solution, check out these tools: Chart.js, Highcharts, Recharts. They are great for creating simple and beautiful pie or bar charts, that can be used on a simple data and can be successfully implemented very quickly. Pre-designed charts are a handy way to get a nice visualisation with little effort (great for MVP). What happens when your requirements change or your product develops? If you ever find yourself in a desire to use two different chart libraries in one project it’s probably the right time to give D3 a shot.

### To Sum-up

D3 is an awesome way to create custom, beautiful, visualisations. In our experience it’s the best data visualisation tool there is – creative, vastly supported and easily implemented in any frontend stack. Data visualisation adds value to almost every product – no matter what’s the main feature of it. We hope that tips shared above will help you get as much as you can from it.**