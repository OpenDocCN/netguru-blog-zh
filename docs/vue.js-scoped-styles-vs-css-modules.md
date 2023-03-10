# Vue.js 范围的样式与 CSS 模块

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/vue.js-scoped-styles-vs-css-modules>

 现代 web 开发中的 CSS 远非完美，这不足为奇。

如今，项目通常非常复杂，鉴于风格的全球性，很容易出现互相冲突的风格，或者隐含地级联到我们以前没有考虑的元素。

我们用来减少主要痛点的最常用的解决方案是引入 [BEM](http://web.archive.org/web/20230220193706/http://getbem.com/) (块元素修改器)方法。然而，它只解决了更大问题的一小部分。

对我们来说幸运的是，社区已经开发了解决方案，可以帮助我们更彻底地处理这个问题。你可能已经听说过 [**CSS 模块**](http://web.archive.org/web/20230220193706/https://github.com/css-modules/css-modules) ， **[风格化组件](http://web.archive.org/web/20230220193706/https://www.styled-components.com/)** ， **[迷人的](http://web.archive.org/web/20230220193706/https://github.com/paypal/glamorous)** 或[**JSS**](http://web.archive.org/web/20230220193706/http://cssinjs.org/)——这些只是我们今天可以添加到项目中的一些最流行的工具。如果你对这个话题感兴趣，你可以查看[这篇文章](http://web.archive.org/web/20230220193706/https://hackernoon.com/all-you-need-to-know-about-css-in-js-984a72d48ebc) - [Indrek Lasn](http://web.archive.org/web/20230220193706/https://twitter.com/lasnindrek) 非常透彻地解释了整个 CSS-in-JS 的概念。

由 [vue-cli](http://web.archive.org/web/20230220193706/https://cli.vuejs.org/) 创建的每一个新的 Vue.js 应用程序都带有两个很棒的内置解决方案:[范围的 CSS](http://web.archive.org/web/20230220193706/https://vue-loader.vuejs.org/guide/scoped-css.html#scoped-css) 和 [CSS 模块](http://web.archive.org/web/20230220193706/https://vue-loader.vuejs.org/guide/css-modules.html#css-modules)。这两种方法各有利弊，所以让我们仔细看看哪种解决方案可能更适合您的情况。

## **作用域样式**

为了让作用域样式正常工作，我们只需给`**<style>**`标签添加一个**作用域的**属性:

通过使用 PostCSS 并将上面的示例转换为下面的示例，它将只将我们的样式应用于同一组件中的元素:

如你所见，拥有良好的作用域样式根本不需要任何努力，而且**也以同样的方式处理作用域标签的样式**。

现在，如果您需要——比方说——更改特定视图中组件的宽度，您可以向它应用一个额外的类，并像平常一样使用作用域样式的所有好处来样式化它:

它将转变为:

再一次——不需要额外的努力，你就可以完全控制布局。

然而，请注意 这个特性的引入有一个缺点——如果你的子组件的根元素有一个类也存在于父组件中，父组件的样式将泄漏给子组件。你可以查看一下 [这个 CodeSandbox](http://web.archive.org/web/20230220193706/https://codesandbox.io/s/l9340n5x99) 来更好的了解这个问题。

虽然不建议也应该避免这样做——但是有些情况下我们需要在子组件内部做一些深入的设计。为了简单起见，让我们假设我们的父组件应该负责 BasePanel 组件的样式头。在作用域样式中，`>>>`组合子(也称为`/deep/`)在这一点上派上了用场。

它将转变为:

简单明了，是吧？**但是请注意，我们刚刚失去了封装**。将在该组件中使用的任何`.title`类(即使是隐含地由孙组件使用)都将受到这些样式的影响。

**CSS 模块**

CSS 模块之所以流行，是因为 React 社区很快采用了这项技术。通过使用 [vue-cli](http://web.archive.org/web/20230220193706/https://cli.vuejs.org/) ，Vue.js 将其功能与易用性和开箱即用支持相结合，将其提升到另一个水平。

现在让我们看看如何使用它:

## 我们不使用`scoped`属性，而是使用`module`。它将告诉 [vue-template-compiler](http://web.archive.org/web/20230220193706/https://github.com/vuejs/vue/tree/dev/packages/vue-template-compiler#readme) 和 vue-cli 的 webpack 配置使用适当的加载器来处理这部分并生成以下 CSS:

它与作用域样式的不同之处在于，所有创建的类都可以通过组件内部的`$style`对象来访问。所以为了应用这个类，我们必须使用类绑定:

这将生成以下 HTML 和相关样式:

第一个好处是，通过查看 HTML 中的这个元素，我们可以立即知道它属于哪个组件。其次，一切都变得非常明确，我们有完全的控制权-没有任何魔法。然而，**在设计 HTML 标签**的样式时，我们必须小心，因为它们在最终的 CSS 中是原样的，这与作用域样式相反，在作用域样式中，即使普通标签也是由唯一的数据属性来限定作用域的。

类似于作用域样式的第二个例子，让我们看看如何在特定的上下文中样式化组件:

它将转变为:

它只是简单地完成工作，没有任何惊喜！此外，因为所有类都可以通过`$style`对象获得，我们现在可以使用 props 将它们传递到我们想要的深度，这使得在子组件的任何位置使用类变得非常容易:

CSS 模块与 JS 有很好的互操作性，它们不会把你限制在类上。使用`:export`关键字，我们还可以将额外的东西导出到我们的`$style`对象。

假设您有一个要开发的图表——您可以将颜色变量保存在 CSS 中，并额外导出它们以在组件中使用:

我在这里仅仅触及了表面——**CSS 模块**的概念要宽泛得多，我鼓励你查看完整的[规范](http://web.archive.org/web/20230220193706/https://github.com/css-modules/css-modules)以了解更多。

**总结**

两种解决方案都非常简单易用，并且在一定程度上解决了相同的问题。那你该选哪个？

作用域样式实际上不需要额外的知识就可以使用并感到舒适。它们的局限性也使它们易于使用，并且能够支持中小型应用程序。

然而，当涉及到更复杂的场景和更大的应用程序时，我们可能希望更加明确，对 CSS 中发生的事情有更多的控制。尽管在一个模板中多次使用`$style`对象可能看起来不那么有趣，但对于它所允许的安全性和灵活性来说，这是一个很小的代价。我们还可以轻松地访问 JS 中的变量(如颜色或断点)，而不必保持单独的文件同步。

你用哪一个？为什么呢？请随意分享您在此过程中遇到的任何其他场景！

I only scratched the surface here - the **CSS Modules** concept is much broader and I encourage you to check out the full [specification](http://web.archive.org/web/20230220193706/https://github.com/css-modules/css-modules) to know more.

## **Summary**

**Both solutions are very simple, easy to use and, to an extent, solve the same issue.** Which one should you choose then?

Scoped styles require literally no extra knowledge to use and feel comfortable with. Their limitations also make them simple to use, and they're capable of supporting small to mid-sized applications.

However, when it comes to more complex scenarios and bigger apps, we probably want to be more explicit and have more control over what’s going on in our CSS. Even though using the `$style` object multiple times in a template might not look so sexy, it’s a small price to pay for the safety and flexibility it allows. We also get easy access to our variables (like colours or breakpoints) in JS, without having to keep separate files in sync.

Which one do you use? And why? Feel free to share any additional scenarios you encountered along the way!