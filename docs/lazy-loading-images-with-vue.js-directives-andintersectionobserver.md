# 使用 Vue.js 指令和 IntersectionObserver 延迟加载图像

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/lazy-loading-images-with-vue.js-directives-andintersectionobserver>

 当我考虑性能和网站如何加载时，我首先想到的是当内容被加载时，页面上出现的最后一个元素是图像。

如今，当谈到性能时，图像(它们在单个页面上的大小和数量)可能是一个主要问题。请记住，一个网站的页面加载时间对转化率有直接影响，这个问题不应该被忽视。

在这篇文章中，我想介绍一种降低网站初始权重的方法。我将演示的是如何在用户看到初始视图时只加载对他/她可见的内容，并且只在需要时才延迟加载所有其余的重要元素(如图像)。

为此，我们需要解决两件事:

1.  如何在第一时间存储我们想要加载的图片的来源而不加载。
2.  如何检测图像何时对用户可见(是必需的)并触发加载图像的请求。

**我们将使用[数据属性](http://web.archive.org/web/20221209120138/https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes)、[交叉观察器](http://web.archive.org/web/20221209120138/https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)和 [Vue.js 自定义指令](http://web.archive.org/web/20221209120138/https://vuejs.org/v2/guide/custom-directive.html)来解决上述问题。**

为了更容易理解，我创建了一个例子，一个随机文章的列表，每篇文章都包含一个简短的描述、图片和文章来源的链接。我们将介绍用于显示文章列表、显示单个文章以及显示(和延迟加载)特定文章的图像的组件的创建过程。

我想展示的大部分内容都是基于显示图像的组件，所以我将展示一个组件的基本示例。我们将在接下来的部分中一步一步地解释，所以现在还不要太深入代码。

**image item . view**

在这个组件中，我们有一个在加载图像时显示的`ImageSpinner`组件，以及一个负责保存图像源并在加载完成时显示图像的`img`标签。

关键的惰性加载逻辑被提取到一个`LazyLoadDirective`中，通过添加`v-lazyload`属性在我们的组件上使用。

*image item . view*

![](img/5b57e077e605cbcfd76e608ab12bcbc1.png)

该组件的脚本部分如下所示:

*image item . view*

如上所述，惰性加载逻辑保存在`LazyLoadDirective`中:

*lazy addictive . js*

![](img/cd6c7211a569ac00a19d3c2427f7cedb.png)

这只是一个工作示例的一部分。你可以在这个 **[Codesandbox 示例](http://web.archive.org/web/20221209120138/https://codesandbox.io/s/5v17x4zr64)** 中找到完整的解决方案。我们将一段一段地分析代码，看看在下一节中实际发生了什么。

**真实世界的例子:创建一个 LazyLoadDirective 并在 ImageItem 组件中使用它**

**1。创建一个基本的 ImageItem 组件**

让我们从创建一个显示图像的组件开始(不涉及延迟加载)。在模板中，我们创建一个包含图像的`figure`标签，图像本身接收一个携带图像源(URL)的`src`属性。

![](img/6f50ac40d0679e9f55057d47ff888c8d.png)

*image item . view*

在脚本部分，我们接收属性`source`,它给出了我们正在显示的图像的源 URL。

*image item . view*

### 这很好——我们渲染了我们想要的图像。但是如果我们让它保持原样，我们将直接加载图像，而不需要等待组件在我们的屏幕上可见。这不是我们想要的，所以让我们进入下一步。

### **2。防止在创建组件时加载图像。**

为了防止图像被加载，我们需要从`img`标签中去掉`src`属性。但是，正如开头所指出的，我们仍然需要在某个地方保存图像源。保存这些信息的好地方是[数据-*](http://web.archive.org/web/20221209120138/https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes) [属性](http://web.archive.org/web/20221209120138/https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes)。

根据定义，[数据-*](http://web.archive.org/web/20221209120138/https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes) [属性](http://web.archive.org/web/20221209120138/https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes)允许我们在标准的语义 HTML 元素中存储信息。

听起来非常适合我们的需要。

*image item . view*

好了，完成后，我们将不会加载我们的图像。但是等等，我们不会加载我们的图像…永远不会！

### 显然，这不是我们想要的。我们希望加载我们的图像，但是在特定的条件下。我们可以通过用保存在`data-url`中的图像源 url 替换`src`属性来请求加载图像。这是容易的部分，我们现在遇到的问题是——我们什么时候应该替换那个`src?`

我们希望当承载图像的组件对用户可见时加载图像。我们如何检测用户是否看到我们的图像？让我们在下一步检查一下。

**3。检测图像何时对用户可见。**

如果你曾经遇到过这样的挑战，你可能最终会使用一些疯狂的、不可思议的 JavaScript，当你完成时，它们看起来并不是很好。例如，我们可以使用事件和事件处理程序来检测滚动位置、偏移值、元素高度和视口高度，并计算图像是否在视口中。这听起来很疯狂，不是吗？如果我们真的需要，我们可能会坚持那个解决方案(不管那会有多难看)，但是那样做对我们网站的性能有直接的影响。每次发生滚动事件时，都会执行这些计算。更糟糕的是，想象几十个图像，每个都必须重新计算它们在每个滚动事件中是否可见。这太疯狂了！

通过使用 **[交叉点观察器 API](http://web.archive.org/web/20221209120138/https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)** 可以解决这种非常低效的检测元素在视口中是否可见的方法。

![](img/14acc3e32336b15bc1a7478236c6e0fd.png)

查看描述，它允许您配置一个**回调**，每当一个元素(称为**目标**)与设备视口或指定元素相交时，就会调用这个回调。

当元素在视口中可见时触发自定义回调函数？听起来像是我们需要的魔咒。

那么，我们需要做些什么来使用它呢？

要使用 **[交点观察者](http://web.archive.org/web/20221209120138/https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)** 我们需要做几件事:

创建新的交叉点观察点

### 观察我们希望延迟加载的元素的可见性变化

当元素在视口中时，加载元素(用我们的`data-url`替换`src`)

一旦元素被加载，停止观察它的可见性变化(`unobserve`)

在 **[Vue](http://web.archive.org/web/20221209120138/https://vuejs.org/)** 中，我们可以使用一个 **[自定义指令](http://web.archive.org/web/20221209120138/https://vuejs.org/v2/guide/custom-directive.html)** 来包装所有这些功能，然后在需要时重用它们。

什么是自定义指令？根据文档，这是获得对元素的低级 DOM 访问的一种方式。例如，改变一个特定 DOM 元素的属性，或者在我们的例子中，改变一个`img`元素的`src`属性。

我们的指令如下所示。同样，我们一会儿会把它分成几个部分，我只是想给你们一个概述。

*lazy addictive . js*

*   我们一步一步来。
*   **[挂钩功能](http://web.archive.org/web/20221209120138/https://vuejs.org/v2/guide/custom-directive.html#Hook-Functions)**
*   允许我们在绑定元素生命周期的特定时刻触发定制逻辑。
*   我们使用一个`inserted`钩子，因为当绑定元素被插入到它的父节点时，它会被调用(这保证了父节点的存在)。因为我们想要观察一个元素相对于它的父元素(或者任何祖先)的可见性，我们需要使用这个钩子。

*lazy addictive . js*

**loadImage 函数**

负责用`data-url`替换`src`值。

在这个函数中，我们可以访问我们的`el`，指令所应用到的元素。我们可以从那个元素中提取出`img`。

我们检查图像是否存在，如果存在，我们添加一个监听器，它将在加载完成时触发一个回调函数。该回调将负责隐藏微调器，并使用 CSS 类向图像添加动画(淡入效果)。

我们添加了第二个监听器，当从我们的源 URL 加载图像失败时，将调用这个监听器。

*   最后，我们用我们想要请求和显示的图像的源 URL 替换我们的`img`元素的`src`(这触发了请求)。
*   *lazy addictive . js*

**handleIntersect 函数**

![](img/2fef6d8bfa8d7c5d309523b559b9830c.png)

这是一个 IntersectionObserver 回调函数，负责在特定条件下触发`loadImage`。

当 IntersectionObserver 检测到元素进入了 viewport 或父 component 元素时，将触发该事件。

它可以访问`entries`，这是观察者和`observer`本身所观察到的所有元素的数组。

*   我们遍历`entries`并检查单个条目是否通过`isIntersecting`对我们的用户可见，如果可见，则触发`loadImage`函数。
*   请求图像后，我们`unobserve`元素(从观察者的观察列表中移除它)。这可以防止图像被再次加载。
*   *lazy addictive . js*
*   **[创建观察者](http://web.archive.org/web/20221209120138/https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API#Creating_an_intersection_observer)** **功能**
*   负责创建我们的 **IntersectionObserver** 并将其附加到我们的元素。

**intersect observer**构造函数接受一个**回调**(我们的 **handleIntersect** 函数)，当被观察的元素经过指定的`threshold`时，该函数被触发，并接受一个携带我们的观察器选项的 **options** 对象。

`options`对象指定了一个`root`——这是我们所关注元素的可见性所基于的引用对象(如果我们传递了`null`，它可以是对象的任何祖先或者是我们的浏览器视窗)。它还指定了一个`threshold`值，该值可以在 0 到 1 之间变化，并告诉我们观察者的回调应该在目标可见性的多少百分比上执行(0 表示一个像素可见，1 表示整个元素必须可见)。

*   在创建了 **IntersectionObserver** 之后，我们使用`observe`方法将它附加到我们的元素上。
*   *lazy addictive . js*
*   **浏览器支持**
*   尽管并非所有浏览器都支持它，但覆盖 73%的网络用户(截至 2018 年 8 月 28 日)听起来已经足够好了。
*   但是，记住我们想要向所有用户显示图像(记住使用`data-url`会阻止图像被加载)，我们需要在我们的指令中再添加一个部分。

我们需要检查浏览器是否支持 IntersectionObserver，如果不支持就触发`loadImage`(这将一次请求所有图像)，如果支持就触发`createObserver`。

*lazy addictive . js*

*   要使用我们新创建的指令，我们需要首先注册它。我们可以通过两种方式实现，全局(在应用程序中随处可用)或本地(在指定的组件级别上)。

*   **全球注册**
*   为了全局注册一个指令，我们导入我们的指令并使用`Vue.directive`方法来传递我们希望用来注册我们的指令的名字和指令本身。这允许我们将`v-lazyload`属性添加到代码中的任何元素。

*main.js*

![](img/df31797954148f0e0f27179ba2149efb.png)

**本地注册**

如果我们只想在特定的组件中使用我们的指令，并限制对它的访问，我们可以在本地注册该指令。要做到这一点，我们需要将指令导入到将要使用它的组件中，并在`directives`对象中注册它。这将使我们能够向该组件中的任何元素添加`v-lazyload`属性。

**7。使用 ImageItem 组件上的指令**

注册我们的指令后，我们可以通过向携带我们的`img`(在我们的例子中是`figure`标签)的父元素添加`v-lazyload`属性来使用它。

*image item . view*

**总结**

延迟加载图像可以显著提高页面的性能。它只允许你在用户可以看到图片的时候加载图片。

![](img/8e6d642265b4b85ab85dc456c9f99e4b.png)

对于那些仍然不相信它是否值得一试的人，我准备了一些原始数据。就拿我们简单的文章列表来说吧。在我进行测试的时候，它有 11 篇文章带有图片(意味着页面上有 11 张图片)。我不认为这是很多的图片，你可以在一秒钟内找到更多的图片。

让我们坚持我们的 11 张图片，检查我们的页面在 *fast 3G* 上的性能，只有第一篇文章可见，没有惰性加载的图片。

不出所料——11 张图片，11 个请求，总页面大小 3.2 MB。

现在，同一个页面上只有第一篇文章可见，还有延迟加载的图片。

![](img/74f1cdc8a6238d3bfe0a6c801fdb32cb.png)

结果:1 个图像，1 个请求，总页面大小 1.4 MB。

通过将这个指令添加到我们的文章中，我们**节省了 10 个请求**，我们**将页面大小减少了 56%**——记住这是一个非常简单的例子。

不再多做评论，让数字自己说话。

If we want to use our directive only in a specific component and restrict the access to it, we can register the directive locally. To do that, we need to import the directive inside the component that will use it and register it in the `directives` object. This will give us the ability to add the `v-lazyload` attribute to any element in that component.

### **7\. Using the directive on the ImageItem component**

After registering our directive, we can use it by adding the `v-lazyload` attribute to the parent element that carries our `img` (the `figure` tag in our case).

![](img/8f60e81354117cb12db5b9bb558ee401.png)

*ImageItem.vue*

### **Summary**

Lazy loading images can **significantly improve your page's performance**. It allows you to load images only when the user can actually see them.

For those still not convinced if it is worth playing with, I prepared some raw numbers. Let’s take our simple articles list. At the moment I was performing that test, it had 11 articles with images (meaning 11 images on the page). I don’t think that is a lot of images, you can probably find bigger number in a second by going to any news page.

Let’s stick to our 11 images and check the performance of our page on *fast 3G* with only the first article visible, without lazy-loaded images.

![](img/8737ed79e8093e1c9c05c62d55d86647.png)

As expected - 11 images, 11 requests, total page size 3.2 MB.

Now, the same page with only the first article visible and lazy-loaded images.

Result: 1 image, 1 request, total page size 1.4 MB.

By adding this directive to our articles we **saved 10 requests** and we **reduced the page size by 56%** - and bear in mind this is a really simple example.

No more comments, let the numbers speak for themselves.