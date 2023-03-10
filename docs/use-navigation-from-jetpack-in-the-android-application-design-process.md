# 在 Android 应用程序设计过程中使用 Jetpack 导航

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/use-navigation-from-jetpack-in-the-android-application-design-process>

 Android 应用程序设计问题

在开发 Android 应用程序的过程中，我必须实现一个定制设计系统，其中:

*   需要从 DeepLink 打开屏幕。

*   仪表盘屏幕有几个按钮可以打开其他屏幕。

*   带工具栏的屏幕包含一个关闭当前屏幕的右键。

*   屏幕只有一个向左返回的箭头按钮，让你回到上一个屏幕(像在 iOS 中一样)。

*   屏幕包含上述两个按钮。

*   UX 设计项目包含一百多个屏幕。

作为一名 Android 开发者，我首先考虑的是:

*   如何在应用中实现导航？

*   用户从 DeepLink 打开应用时，如何实现导航？

*   UX 设计是否涵盖了所有可能的应用工作流程？

*   从用户的角度来看，UX 的设计是否高效？

回答第一个问题，我开发了类似 NavigationHelper 或者 Navigator 类的东西来管理活动中片段之间的导航。该类的对象需要活动上下文来启动另一个活动或 FragmentManager 来管理活动内部的片段。但是这个对象解决了来自 DeepLink 的导航吗？为了捕获一个深度链接，我们需要实现以下内容:

*   在你的货单里设置一个合适的<意向过滤器>作为入口点。

*   考虑为目标活动设置 android:launchMode="singleTask "以避免同时运行您的活动的多个副本。

*   处理意图。

*   将深度链接与预定义的正则表达式相匹配，以将用户导航至正确的页面/内容。

接下来我面临的问题是:

*   低效的用户界面流程(例如，在应用程序中做某件事需要太多的点击，在应用程序的某个地方需要浏览太多的屏幕等等。)

*   缺少或不完整的应用程序工作流路径。

这两个问题都是 UX 设计问题，需要在开发商发现存在后，与 UX 设计师一起解决。为此，你不必联系另一家 [UX 设计机构](/web/20221004133318/https://www.netguru.com/services/ux-design)。Netguru 会帮你做的。

## Jetpack 的导航会有所帮助

为了简化这些问题，谷歌在 2018 年 5 月的年度谷歌 I/O 活动上展示了来自 Jetpack 的导航。导航组件是“某种东西”,它让我们能够摆脱这些 Android 应用程序设计问题，或者至少将它们减少到最低限度。首先，导航组件是一个表示存储为 XML 文件的图形的对象。Android Studio 提供了这个文件的 v 可视化来表示一个活动的导航流程。下面是一个在 Android Studio 中可视化的导航图的例子。

![blog 1](img/8d130996fafef300da159121fdd6944a.png)

上图中:

*   图中所代表的片段都是目的地。

*   目的地通过动作连接。每个动作在导航图线中用一个箭头表示。箭头显示了我们从源到目的地的导航方向。

如图所示，例如 *设置片段* 没有连接。工作流路径缺失或不完整。屏幕之间没有循环动作。上面的例子显示了在 Android Studio 中检查所有不完整的路径或低效的 UI 流是多么容易。导航组件简化了深度链接的处理。我必须处理来自其他应用程序的深层链接来触发特定的屏幕。导航组件在导航图中提供静态上下文。下面是一个片段，它定义了目的地的 URL。在这个例子中，要传递的参数用花括号描述。

```
<fragment
   android:id="@+id/android"
   android:name="com.example.android.codelabs.navigation.DeepLinkFragment"
   android:label="@string/deeplink"
   tools:layout="@layout/deeplink_fragment">

   <argument
       android:name="myarg"
       android:defaultValue="Android!"/>
   <deepLink app:uri="www.example.com/{myarg}" />
</fragment>
```

工具栏左右两边按钮的问题可以通过以下方式解决:

*   对于带箭头的左键返回到上一个屏幕我们在下面呈现的 Activity 类中有回调方法:

    ```
    override fun onSupportNavigateUp(): Boolean {
        return NavigationUI.navigateUp(drawerLayout,
               Navigation.findNavController(this, R.id.my_nav_host_fragment))
    } 
    ```

该项目的 ID 将在导航图中用于导航到该目的地。下面的代码片段显示了与上述商品 ID 相关的导航图的目的地部分。

```
<fragment
   android:id="@+id/shopping_cart"
   android:name="com.example.android.codelabs.navigation.CartFragment"
   android:label="CartFragment"
   tools:layout="@layout/cart_fragment" />
```

## 原型制作过程

在使用导航组件时，我发现导航图形预览可以呈现布局的资源。为此，我必须将 UX 设计中的图像设置为每个片段布局的背景属性。



![blog 6](img/b73db4c9432d2bb4458d71ca0d45de9f.png)

最后，我在导航编辑器中获得顶级应用程序导航的用户流。在一个有超过一百个屏幕的应用程序中，避免迷路是非常有用的。完成原型的最后一步是处理来自所有目的地的按钮动作，使应用程序可点击。为此，我必须在布局中添加一个按钮(例如 ImageView，button)，然后在源代码中应用 onClick 处理。下面的代码片段显示了如何从导航图中执行 XML 操作。

```
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    view.findViewById<view>(R.id.next_button).setOnClickListener(
            Navigation.createNavigateOnClickListener(R.id.next_action)
    )
}</view> 
```

当这个过程在 UX 设计的所有屏幕上完成后，我就有了一个完全可点击的 Android 应用程序的原型。

## 总结

所描述的原型化过程可以视为 Android 应用开发的第一阶段。当一个 Android 开发者不得不处理一个导航乍一看并不直观的 UX 设计时，这个过程可以用来处理应用程序中的顶级导航。

优点 和 缺点

*   流程会根据 UX 设计 显示应用工作流中缺少哪些路径
*   加快研发进程争取 th e 最小可行产品(MVP)
*   原型顶层导航将展示它如何在真实设备上工作
*   在导航编辑器 中轻松快速地更改动作及其转场属性

*   从 Jetpack 导航可以将动作链接到一个活动的目的地。导航编辑器将只显示单个活动的应用程序工作流。
*   导航编辑器不适用于通过工具栏执行源目的地布局和目标目的地布局之间的过渡的设计系统(从活动导航到活动)

* * *

照片由 [阿尔瓦罗·雷耶斯](http://web.archive.org/web/20221004133318/https://unsplash.com/photos/qWwpHwip31M?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [Unsplash](http://web.archive.org/web/20221004133318/https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)