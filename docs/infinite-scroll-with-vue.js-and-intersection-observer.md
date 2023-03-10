# 带有 Vue.js 和交叉点观察器的无限滚动

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/infinite-scroll-with-vue.js-and-intersection-observer>

 如果我们看看 web 应用程序，大多数应用程序背后的想法是从数据库中获取数据，并以最好的方式呈现给用户。当我们处理数据时，有些情况下最好的呈现方式意味着创建一个列表。

根据数据量及其内容，我们可能会决定一次显示所有内容(很少)，或者只显示更大数据集的特定部分。只显示部分现有数据的主要原因是，我们希望尽可能保持应用程序的高性能，并避免加载/显示不必要的数据。

如果我们决定以块''的形式显示我们的数据*,我们需要一种在集合中导航的方法。*

**浏览数据集**的两种最常见方式是:

*   **分页** -将一组数据分割成**特定数量的页面的技术-** 它可以避免用户被一个页面上的大量数据淹没，并允许用户浏览页面
*   **无限滚动**—**在用户向下滚动页面时持续加载内容**的技术，消除了分页的需要

我用一个随机文章列表创建了一个例子，每篇文章都包含一个简短的描述、图片和一个到文章来源的链接。我们将创建一个负责显示该列表的组件，并在到达现有集合的末尾时触发获取附加文章的操作。这意味着创建一个无限滚动组件。

让我们看看如何建立它。

## 步骤 1:在 [Vue](http://web.archive.org/web/20221007191217/https://vuejs.org/) 中创建 ArticlesList 组件

让我们首先创建一个显示文章列表的组件(但是还没有无限滚动)。我们称之为*文章列表*。在组件模板中，我们将遍历文章集，并将单个文章项传递给每个 *ArticleItem* 组件。

![carbon (40)](img/db64507e3eb5625701127e7f38135888.png)

在组件的脚本部分，我们将初始文章设置为一个空数组，并用从安装了钩子的[上的 API 获取的数据填充该数组。在这个阶段，我们只获取新闻提要的第一页。这将为我们提供前三篇文章的列表。](http://web.archive.org/web/20221007191217/https://vuejs.org/v2/api/#mounted)

步骤 2:创建 loadMore 方法

![carbon (40) (1)](img/f5fb9c25250110f260450b80e596a975.png)

现在我们需要创建一个方法来加载下一页，我们还需要保存一个对当前页面的引用，并在加载新数据之前增加该值。

## 我们将在数据中添加一个页面属性来保存上次加载页面的值。在加载新文章之前，在 *loadMore* 方法中，我们增加页面值并从 API 获取下一批数据。收到新数据后，我们将现有的文章数组与包含下一页文章的新数据合并。

现在我们有了 *loadMore* 方法，但是我们没有在任何地方触发它。我们需要找到一种方法来检测用户是否到达了列表的末尾，然后触发 *loadMore* 。

为了做到这一点，我们需要做两件事:

![carbon (41) (1)](img/5608134aa85ff532fa5625d131d6fb45.png)

**创建一个组件**,它将呈现在列表中最后一个元素的下面。

**检测** **当**那个组件**对用户变得可见**并且**触发请求**装载更多的物品。

我们继续吧！

1.  步骤 3:创建触发器组件
2.  我们的*触发器*组件不必包含任何内容，除了一个如下所示的“ *<跨度>’*标签。

Let’s proceed!

## Step 3: Create a Trigger component

我们将在 *ArticlesList* 组件中的 *ArticleItems* 下呈现该组件。

![carbon (42) (1)](img/780e2cfa04bafdd870adeb2a66f1e868.png)

这是容易的部分，现在我们需要检测组件何时对用户可见，并触发加载更多文章的请求。我们如何做到这一点？

步骤 4:检测触发器组件何时对用户可见

![carbon (43)](img/e2fc5cb7760a32fca2a0d91b68eac6a3.png)

有很多方法可以让**使用 JavaScript 确定一个元素何时出现在视图**中，也许你甚至创建了一些*定制解决方案*来实现这一点。我敢打赌，虽然你对这个结果不太满意，但你还是坚持了下来，因为没有更好的办法。

信不信由你，但是那个**更好的方式存在**，它叫 **[路口观察者](http://web.archive.org/web/20221007191217/https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)** 。这提供了一种非常有效的方法来检测元素在视口中是否可见。具体来说，它允许您配置一个**回调**，当一个元素(称为**目标**)与设备视口或指定元素相交时触发该回调。

## **那么，我们需要做些什么来使用它呢？几样东西:**

创建新的交叉点观察点

观察元素的可见性变化

当它变得可见时触发事件

*   如果组件之前的组件被破坏，则停止观察可见性变化(断开连接)
*   我们现在将把所有的逻辑放入我们的*触发器*组件中。
*   trigger the event when it becomes visible
*   stop watching for visibility changes (disconnect) if the component before that component is destroyed

让我们把这段代码分成几部分，解释一下发生了什么。

开始时，我们允许传递一个' *options'* '对象作为组件的道具。这个'*选项'*对象允许我们配置我们的[交集观察者](http://web.archive.org/web/20221007191217/https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)(特别是观察者的**回调**将被调用的情况)。它包含(但不限于)以下道具:

![carbon (43) (1)](img/a779115d2411dec0e4c6f143faea4f57.png)

**root** 作为我们的引用对象，我们用它来确定被监视元素的可见性。如果我们传递 null，它可能是对象或浏览器视窗的任何祖先。

**threshold** 值，可以在 0 到 1 之间变化，告诉我们观察者回调应该在目标可见性的多少百分比上执行，0 表示一个像素可见，1 表示整个元素必须可见。

At the beginning we allow to pass an '*options'* object as a prop to the component. This '*options'* object allows us to configure our [Intersection Observer](http://web.archive.org/web/20221007191217/https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) (specifically circumstances under which the observer's **callback** will be invoked). It contains (but is not limited to) following props:

*   **root** as our reference object, which we use to base the visibility of our watched element. It might be any ancestor of the object or our browser viewport if we pass null.
*   在'*数据'*中，我们将*观测器*的初始值设置为*空值*。当我们创建[交叉点观察器](http://web.archive.org/web/20221007191217/https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)时，这个值将被改变。

![carbon (44) (1)](img/f39845d73639924e1d3334b4a024498e.png)

核心逻辑存在于组件的[钩子函数](http://web.archive.org/web/20221007191217/https://vuejs.org/v2/guide/custom-directive.html#Hook-Functions)中。该函数允许我们在绑定元素生命周期的特定时刻触发自定义逻辑。在我们的例子中，我们将使用[安装的](http://web.archive.org/web/20221007191217/https://vuejs.org/v2/api/#mounted)吊钩。

在那里，我们[创建一个交叉点观察器](http://web.archive.org/web/20221007191217/https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API#Creating_an_intersection_observer)。*intersect observer*构造函数接受一个回调函数( *handleIntersect* 函数)，当被观察元素通过指定的*阈值*和携带我们的观察者*选项*的 options 对象时，该函数被触发。它还可以访问[条目](http://web.archive.org/web/20221007191217/https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry)，这是观察者所观察的所有元素的数组。

![carbon (45) (1)](img/37be93892aa97f490ebb05fced202145.png)

然后，在创建了*交叉点观察器*之后，我们使用[观察器](http://web.archive.org/web/20221007191217/https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver/observe)方法将它附加到我们的组件上。

The core logic lives in the component’s [hook function](http://web.archive.org/web/20221007191217/https://vuejs.org/v2/guide/custom-directive.html#Hook-Functions). That function allows us to fire a custom logic at a specific moment of a bound element lifecycle. In our case we will use [mounted](http://web.archive.org/web/20221007191217/https://vuejs.org/v2/api/#mounted) hook.

In there we [create an intersection observer](http://web.archive.org/web/20221007191217/https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API#Creating_an_intersection_observer). The *IntersectionObserver* constructor accepts a callback (*handleIntersect* function) that is fired when the observed element passes the specified *threshold* and the options object that carries our observer *options*. It also has access to [entries](http://web.archive.org/web/20221007191217/https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry), which is an array of all elements that are watched by the observer.

在 *handleIntersect* 函数中，当满足某些条件时，我们发出一个*触发 intersect*事件。具体来说，当*交集观察者*检测到元素进入视口或父组件元素时，它被触发。该事件将被父组件捕获，并用于触发 *loadMore* 功能。

![carbon (46) (1)](img/c044c9228f6555f70009be15a60c15f9.png)

我们需要对我们的*触发器*组件做的最后一件事是在它被破坏之前停止观察组件交集。为了做到这一点，我们再次使用[钩子函数](http://web.archive.org/web/20221007191217/https://vuejs.org/v2/guide/custom-directive.html#Hook-Functions)，具体来说就是在销毁钩子之前使用[。](http://web.archive.org/web/20221007191217/https://vuejs.org/v2/api/#beforeDestroy)

![carbon (47) (1)](img/b00caf29f853eed2a00602561e899765.png)

第五步:捕捉 triggerIntersected 事件并获取额外的文章。

缺失的部分是我们的*触发器*组件在视窗中变得可见的时刻和我们负责加载更多文章的 *loadMore* 方法之间的连接。

当组件进入视口时，我们的*触发器*组件创建一个*交叉点观察器*和[发出](http://web.archive.org/web/20221007191217/https://vuejs.org/v2/guide/components.html#Sending-Messages-to-Parents-with-Events)事件。我们现在需要做的是在 or *ArticlesList* 组件中捕获该事件。

![carbon (48) (1)](img/3c6f284e82e1850623dea18b851a93b9.png)

我们使用[v-on](http://web.archive.org/web/20221007191217/https://vuejs.org/v2/guide/events.html#Listening-to-Events)指令(或@速记)来实现，这需要附加到我们的 *触发器* 组件上。这将触发*loadMore*方法每次 *触发* 组件发出 *触发 intersect*事件(每次组件进入视口)。

摘要

由于无限卷轴可能听起来像是所有列表的解决方案，**它不是所有情况下的最佳解决方案**，它可能看起来很花哨，但在某些情况下，它可能弊大于利。

**不建议用于目标导向的寻找任务**，要求人们定位特定内容或比较选项。设想寻找出现在搜索结果的特定页面上的特定搜索结果。在这种情况下使用无限滚动很难找到它，因为我们需要多次触发无限滚动来到达我们想要的页面并找到我们的项目。

在这种情况下，分页允许我们通过两次点击(一次点击页面，另一次点击项目)找到项目(只要我们记得它在哪个页面)。

![carbon (49)](img/041e0b360124a5fd517807f2f1334cdd.png)

**无限滚动**显示了其对于不断流**的**内容的潜力，并且具有**相对扁平的结构**(每个项目在**层次结构**的同一级别，并且具有**类似的对用户感兴趣的机会**)。例如，新闻流。在这种情况下，我们是在探索新闻，而不是寻找具体的东西。我们不需要知道新闻的确切位置，也不需要知道我们滚动了多少才能看到特定的新闻，这使得用户体验更加引人入胜，并且省去了我们每次到达当前页面末尾时点击**下一个**的努力。****

 **## Summary

As the infinite scroll may sound like a solution for all lists, **it is not the best solution in all cases**, it may look fancy, but in some cases it can cause more harm than good.

**It is not recommended for goal-oriented finding tasks**, requiring people to locate specific content or compare options. Imagine looking for a specific search result that appears on a certain page of the search results. Using infinite scroll in this case makes it hard to find it as we need to trigger the infinite scroll many times to reach our desired page and find our item.

Having pagination in that case allows us to find the item (as long as we remember at which page it was) in literally two clicks (one for page, second for item).

**Infinite scrolling** shows its potential for **content that streams** **constantly** and has a **relatively flat structure** (each item is at **the same level of hierarchy** and has **similar chances of being interesting** to users). For example, a stream of news. In that case we are exploring the news rather than looking for something specific. We do not need to know where exactly the news is and how much we scrolled to get to a specific news, it makes the user experience more engaging in that case and saves us the effort of clicking **next** each time we reach the end of current page.**