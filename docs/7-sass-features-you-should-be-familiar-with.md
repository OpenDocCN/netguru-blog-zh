# 你应该熟悉的 7 个 Sass 特性

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/7-sass-features-you-should-be-familiar-with>

 像 Sass 这样的预处理程序在我们的 RoR 世界中被广泛使用，它使得编写 CSS 变得更加容易和整洁。大多数 Rails 开发人员都知道嵌套、引用选择器、变量、混合或扩展指令等优点。但是 Sass 远不止这些！

![](img/b9c046de052d6337a7aebe4c7baa7373.png)

在这篇博文中，我将介绍我在日常工作中使用的可能有用的 Sass 特性。

## 1.参考符号(&)

您可能熟悉引用符号，它允许您像这样引用父元素:

 `注意，你也可以把这个符号放在一个选择器后面，这对于解决一些 IE 相关的问题很有用:

 `## 2.部分和@import 指令

在 rails 世界中存在一种约定，即向 body 标签或 site #main 标签添加控制器名称和控制器动作。这当然很好，但有时开发人员倾向于以错误的方式反映 css 中的 html 结构，这可能会导致混乱和过度限定选择器的问题:

 `我发现最好避免嵌套选择器，以使代码更加模块化。我个人认为，嵌套不应该深于第三层。我也喜欢把我的代码分成更小的粒子，然后像这样分组:

 `Sass 代码将评估成这个简单的 css:

 `此外，这种方法有助于抽象和保持其他代码位分开(用于混合、设置(定义的变量)等)。)

## 3.插入文字

我们可以在变量中定义一个元素，并将其插入到 sass 代码中。当您将模块保存在单独的文件中时，这很有用:

 `请注意，只有当我们想要将元素嵌套在彼此内部时，这才是好的。就个人而言，我喜欢 BEM 语法，更喜欢使用缩进来“嵌套”选择器，以反映 html 结构。

 `你可以在 Harry Roberts 的博客[这里](http://web.archive.org/web/20221004135648/http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)阅读更多关于 BEM 的内容。

## 4.@内容指令

从 SASS 3.2.0 开始，我们可以将一个代码块传递到一个 mixin:

 `这在内容取决于您的战斗对象的情况下非常有用:

 `当然，当你在为设备和分辨率而挣扎时，你可以在这里找到更多关于这个主题的内容。

## 5.%占位符

* *自 SASS 3.2.0 起，占位符在您希望编写需要扩展的样式，但不希望在输出 css 样式中看到基本样式时非常有用(与扩展类相反)。

 `Ian Storm Taylor 提到的占位符用法的一个很好的例子是 OOCSS - media 对象的“金童”。你可以在这里阅读他关于 OOCSS 和使用占位符[的文章。](http://web.archive.org/web/20221004135648/http://ianstormtaylor.com/oocss-plus-sass-is-the-best-way-to-css/)

## 6.功能

Sass 有一套内置的函数，其中一些与颜色有关，一些与字符串有关，一些与数字有关。

 `还有一个“如果”函数:

参见 [CodePen](http://web.archive.org/web/20221004135648/http://codepen.io/) 上达维德 woźniak([@达维德](http://web.archive.org/web/20221004135648/http://codepen.io/dawidw))的笔 [mBAps](http://web.archive.org/web/20221004135648/http://codepen.io/dawidw/pen/mBAps)

如果我们切换到主分支，我们可以注意到我们为下一个 sass 3.3 版本添加了一些新的功能，即 to_lowe_case、to_upper_case、str_slice、str_length 等。我们也有函数来操作像 join，zip，index 或 nth 这样的列表。

你也可以这样定义你自己的函数:

 `## 7.列表和@each 指令

列表是 Sass 的一个老特性，单靠它们自己做不了多少事情。

 `然而，当与列表函数(join、lenght、nth、append)和@each 指令结合使用时，事情变得更加有趣，列表用法的完美例子是 sprite 图标。

 `Dale Sande 给出了更高级的例子，可以在他的博客文章中找到。

如果你好奇接下来会发生什么，我强烈推荐大卫·沃尔什关于萨斯的未来的文章。

在 reddit [r/webdev](http://web.archive.org/web/20221004135648/http://www.reddit.com/r/webdev/comments/1lkn46/7_sass_features_you_should_be_familiar_with/) 加入讨论

[来自我们博客的其他 CCS 相关文章](http://web.archive.org/web/20221004135648/https://www.netguru.com/blog/topic/software-development)``````````````