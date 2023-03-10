# 安卓可折叠手机-你准备好了吗？

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/android-foldable-phones-are-you-ready>

 可折叠手机越来越受欢迎。各家公司都在竞相尽快推出最佳的可折叠体验。这对我们有什么好处？机遇和陷阱的混合体，^^！

来源 https://developer.android.com/guide/topics/ui/foldables

在可折叠成为一件事情之前，困扰开发人员的最常见的配置更改是方向更改。然后，在 Android 7.0 中引入了多窗口，将自己添加到需要处理的事情堆中。现在，随着可折叠手机的出现，我们面临着另一个配置变化可能被触发的问题。

每个人都听说过并面临着与方向变化相关的挑战，因此我们将重点关注多窗口和屏幕尺寸的变化——这些选项将在可折叠手机中更经常使用。 [Android 10 引入了一些重大变化](http://web.archive.org/web/20221007154936/https://developer.android.com/about/versions/10/behavior-changes-10#foldables) ，我们就称之为现状。

正如我提到的，自 Android 7.0 以来，多窗口就一直伴随着我们，但我认为当可折叠手机变得更受欢迎时，它将会更加闪耀。那我们需要知道些什么呢？

多窗口模式下所有窗口都处于恢复状态(Android 10)。在此之前，只有一个活动(用户关注的活动)处于恢复状态，而其他活动都处于启动状态，但没有恢复。增加了一个新的生命周期事件(Android 10)，它被称为[*【ontopucedactivitychanged(boolean)*](http://web.archive.org/web/20221007154936/https://developer.android.com/reference/android/app/Activity#onTopResumedActivityChanged(boolean))。我们可以用它来检测用户实际关注的是多个活动中的哪一个。

当你使用多窗口模式时，所有这些变化都可能发生:屏幕大小，屏幕大小，屏幕布局，方向。就像常规的方向更改一样，您可以通过向 configChanges 清单选项添加适当的属性来自行处理它，或者系统将通过为特定配置适当添加资源来为您处理它。

您可以定义应用程序在多窗口模式下启动时的窗口和定位。

## **可折叠屏幕**

来源走吧数码/三星

正如我之前提到的，可折叠屏幕只是另一种配置变化的情况。

我们得到了两个不同的屏幕，当用户折叠/展开时，设备屏幕从一个转换到另一个。那么说到这个，我们应该知道/关心什么呢？

说到 [Android](/web/20221007154936/https://www.netguru.com/blog/android-accessibility-tools-for-users) 配置选项:*Android:screen orientation = " portrait "**估计大家都记得这行代码。每当有人询问在纵向/横向转换过程中应该如何处理方向变化时，你都可以在 Stackoverflow 上找到它。这只是锁定了屏幕方向，使你的应用程序只能在纵向模式下使用。你避免了方向的改变和它可能带来的任何问题。作为交换，你牺牲了应用程序的响应性和沉浸感，降低了对用户的吸引力。当然，并不是每个 app 都应该支持方向改变，因为有时候，这只是大量的工作换来了一个无论如何都不会被用户注意到的罕见用例。*

当可折叠手机开始发挥作用时， [处理配置更改](http://web.archive.org/web/20221007154936/https://developer.android.com/guide/topics/resources/runtime-changes) 而不锁定其应用程序的开发人员在支持可折叠手机方面的工作会更少。如果你已经定义了 [不同屏幕尺寸的资源](http://web.archive.org/web/20221007154936/https://developer.android.com/training/multiscreen/screensizes) ，你就是大赢家^^.

当然，有一个属性可以用来禁止你的应用在可折叠手机过渡期间调整大小:*resizeableActivity = false*

实际情况是这样的:

*   **resizeableActivity**= false

*   **resizeableActivity** =真

你注意到它们之间有什么特别的不同吗？这个应用程序部分处理了尺寸变化，我们可以注意到这两种情况之间的一些细微差异。

*   当涉及到滚动位置和显示网格的数量时，应用程序处理配置更改。
*   底部工作表占位符的状态不会被保存

当 resizeableActivity 为 false 时，应用程序的功能和以前一样:屏幕变大了，但我们的应用程序保持不变，没有发生配置更改。

在两次配置更改之间会保存滚动位置，因此在两种情况下，用户都可以从离开的位置继续滚动。

不处理保存底部工作表的状态，因此当标志打开时，状态会在转换过程中丢失。

因此，如果您不处理配置更改，请确保在您的应用程序中关闭此标志，以避免一些令人讨厌的意外！在上面显示的案例中，一个不利的方面是缺少底层。也就是说，大多数应用程序不会如此微妙——配置更改可能会导致一些奇怪的行为，甚至在某些情况下会崩溃。

来源 https://developer . Android . com/guide/topics/ui/foldables # positions

除了最终状态——折叠/展开——您还可以支持其他 [状态](http://web.archive.org/web/20221007154936/https://developer.android.com/reference/androidx/window/DeviceState#constants_2) 并对其做出反应。为了这样做，你需要添加一个新的 jetpack [窗口管理器库](http://web.archive.org/web/20221007154936/https://developer.android.com/jetpack/androidx/releases/window) 。

如果你想要更多关于折叠事件的信息，你可以监听一个传感器事件，它将产生关于当前 [铰链角度](http://web.archive.org/web/20221007154936/https://developer.android.com/guide/topics/ui/foldables#hinge_angle) 的信息

## **测试**

购买一部可折叠手机来让你的应用程序为新功能做好准备，这是一个有点昂贵的选择。但是不用担心。使用 Android Studio 3.5 及以上版本，您可以模拟可折叠手机。

![width=](img/04bd6a6b4aca8ebd450d2d04802d9d78.png)

除了正常的 Android 模拟器，它还有额外的折叠/展开按钮。

如果这还不够，你还想测试那些铰链角度功能，你需要抓取 [Android Studio 预览版](http://web.archive.org/web/20221007154936/https://developer.android.com/studio/preview) 并在 Android 11 上运行可折叠设备模拟器。然后，您将看到传感器类别中的附加选项和附加模拟器图像。

这就是它的作用

我们的应用程序不太处理水平折叠情况^^

如你所见，铰链没有居中，我们的视角被切错了位置。但是不要担心，窗口管理器库会帮助你的！

**窗口管理器库**

## 正如我前面提到的，窗口管理器库给你检查折叠状态的能力，也可以帮助我们解决切割铰链的问题——我们可以检查铰链的位置。我将使用来自 Android 资源库的 [窗口管理器示例](http://web.archive.org/web/20221007154936/https://github.com/android/user-interface-samples/tree/master/WindowManager) 。 **从窗口管理器中，我们可以得到关于显示特性的信息，描述为:**

**显示特性是位于设备显示面板内的独特物理属性。它会侵入应用程序窗口空间，并造成视觉失真、视觉或触摸不连续、使某些区域不可见或在屏幕空间中造成逻辑分割线或分离。**

> *#TYPE_FOLD*
> 
> *#类型 _ 铰链*
> 
> 在这个示例中，我们可以找到一个 SplitLayout 如何使用 DisplayFeatures 实现视图拆分的示例。我们可以假设谷歌将在未来为我们提供一些不错的工具来帮助我们开发可折叠的应用程序，例如像这个展示应用程序中显示的 SplitLayout 这样的东西，当然，更优化和更高级。

SplitLayout 包括上面可以看到的另外两个视图:内容和控件。它找到铰链的位置并划分显示屏，以便正确测量两个新容器，并定位和测量内容和控制内部布局。

这就是水平折叠的样子——对于控件/内容视图来说，这是一个非常好的用例。

正如你所看到的，有很多新的可能性和用例可以利用——设计师和开发人员可以大放异彩的全新领域。所以，做好准备，跳上折叠火车吧！^^