# Android 切片介绍-第二部分

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/introduction-to-android-slices-part-ii>

 在[之前关于 Android 切片的帖子](/web/20220924171136/https://www.netguru.com/blog/introduction-to-android-slices)中，我们看了一下它们的可能性，创建了我们的第一个切片，并在不同的模式下进行了测试。在本文中，我们将重点关注更高级的片，它们可以采取应用程序内的动作，并从我们的应用程序中获取数据。让我们来看看互动和动态切片。

## 创建交互切片

众所周知，交互切片用于处理不同的输入动作。我们自己造一个吧。我们将创建一个切片，它将允许我们在单击时显示吐司，并将能够切换我们的 Wi-Fi 状态。我们开始吧！

## 视角

首先，我们将关注我们的切片看起来会是什么样子。没有什么真正特殊的，我们将只有两行-一个与“让我烤面包”的文本，第二个与切换负责启用和禁用我们的 Wi-Fi。构建切片的一段代码可以是这样的:

```
private fun createInteractiveSlice(context: Context, sliceUri: Uri): Slice {
    return list(context, sliceUri, ListBuilder.INFINITY) {
  row {
  title = "Make me a Toast!"
  }    row {
  title = "Wi-Fi"
  subtitle = "Enabled"   }
 } }
```

这段代码将给出如下所示的切片:

![Screenshot_1540975225](img/9da3cb8cc780a8229490039b0788d6e2.png)

当然，我们这里少了一个开关、动作和动态字幕，它们会根据 Wi-Fi 状态相应地改变。我们将添加检查我们的 WiFi 状态第一-这不会真的很难，我们将使用良好的旧 WifiManager。

如果我们完成了这一步，下一步我们应该做的是制定行动。

行动

```
private fun createInteractiveSlice(context: Context, sliceUri: Uri): Slice {
    val wifiManager = context.applicationContext.getSystemService(Context.WIFI_SERVICE) as WifiManager
    val isWifiEnabled = wifiManager.isWifiEnabled
  val wifiSubtitle = if (isWifiEnabled) "Enabled" else "Disabled"    return list(context, sliceUri, ListBuilder.INFINITY) {
  row {
  title = "Make me a Toast!"
  primaryAction = createToastAction()
        }    row {
  title = "Wi-Fi"
  subtitle = wifiSubtitle
            primaryAction = createWiFiToggleAction(isWifiEnabled)
        }
 } }
```

就我们的切片视图而言，我们已经完成了。现在我们将暂停一会儿，看看我们的应用程序如何处理动作。

## 为了能够对某些动作做出反应，我们应该创建一个广播接收器。这将非常简单，因为它将只负责检索意图和显示吐司:

如你所见，这很简单:当我们收到一个意图时，我们检查它是否包含适当的动作，然后我们展示祝酒词。

```
override fun onReceive(context: Context?, intent: Intent?) {
    intent ?: return
  context ?: return   if (intent.action == TOAST_ACTION) {
        Toast.makeText(context, "Here is your Toast.", Toast.LENGTH_LONG).show()
    }
}

companion object {
    const val TOAST_ACTION = "toast_action" }
```

现在，我们已经完成了应用程序中的动作处理，但是我们的切片仍然不会触发任何动作。如何才能做到？当然，Slice API 包含一些方便的解决方案叫做 **Slice Actions** 。让我们跳到 SliceProvider 中的 createToastAction()。

切片动作基本上是带有一些附加数据的挂起内容的包装:  

如你所见，在创建 SliceAction 时，我们需要提供一个待定的意向、图标和动作标题。这就是我们需要做的一切。写完这段简单的代码后，我们就有了点击时显示吐司的切片。这并不难，对吧？让我们的 WiFi 切换行动吧！

动作片支持不同的预定义动作。肘节就是其中之一。要创建它，我们可以使用 SliceAction 类的另一个静态方法:createToggle。这是一个非常方便的方法，可以为我们创建一个动作。我们只需提供适当的待定意向。请看下面的代码片段:

```
private fun createToastAction(): SliceAction {
    val intent = Intent(context, MyBroadcastReceiver::class.java).apply {
  action = MyBroadcastReceiver.TOAST_ACTION
  }    return SliceAction.create(
            PendingIntent.getBroadcast(context, 0, intent, 0),
            IconCompat.createWithResource(context, R.drawable.ic_wb_sunny_black_24dp),
            ListBuilder.SMALL_IMAGE,
            "Make Toast"
  )
}
```

很简单，对吧？但是，如果我们没有任何指示器来指示我们的触发器处于哪种状态，我们将如何在广播接收机中处理它呢？随着帮助来了非常酷的功能片 API:当一些行动被触发，片将更新你的意图与所有必要的数据，如滑块值，切换状态等。让我们看看如何在我们的广播接收机中检索这些数据:

```
private fun createWiFiToggleAction(isWiFiEnabled: Boolean): SliceAction {
    val intent = Intent(context, MyBroadcastReceiver::class.java).apply {
  action = MyBroadcastReceiver.WIFI_TOGGLE_ACTION
  }    val pendingIntent = PendingIntent.getBroadcast(context, 0, intent, 0)
    return SliceAction.createToggle(pendingIntent, "Wi-Fi Action", isWiFiEnabled)
}
```

正如我们所看到的，我们正在检查我们的意图是否有一个适当的动作，然后我们可以很容易地通过 Slice 类的静态字段获得包含我们的切换状态的额外信息。正如我前面提到的，这个值默认是在动作被调用后添加的，所以我们不必担心自己传递某种数据。

```
if (intent.action == WIFI_TOGGLE_ACTION) {
    val isWiFiTurnedOn = intent.getBooleanExtra(Slice.EXTRA_TOGGLE_STATE, false)
    val wifiManager = context.applicationContext.getSystemService(Context.WIFI_SERVICE) as WifiManager

    wifiManager.isWifiEnabled = isWiFiTurnedOn
}
```

更新视图

在交互切片的情况下，我们应该做的最后一件事是更新我们的视图。正如您所见，无论我们启用还是禁用 WiFi，我们的切片视图都不会更新。我们该拿这个怎么办？我们可以创建另一个广播接收器来处理任何 wifi 网络状态变化，然后更新我们的切片。整个过程可能听起来有点复杂，但确实很简单。我们的 WiFiBroadcastReceiver 会是这样的:

记得在 AndroidManifest.xml 文件中包含您的 WifiBroadcastReceiver 的正确配置！

```
override fun onReceive(context: Context?, intent: Intent?) {
    val action = intent?.action
  if (action == WifiManager.NETWORK_STATE_CHANGED_ACTION) {
        context?.contentResolver?.notifyChange(MySliceProvider.INTERACTIVE_SLICE_URI, null)
    }
}
```

这里最重要的部分是通过调用内容解析器上的 notifyChange 方法来更新我们的切片内容。我们只需要提供我们的切片 URI，之后，托管我们切片的应用程序将重新创建它。现在我们的切片完成了:

**Remember to include proper configuration of your WifiBroadcastReceiver into AndroidManifest.xml file!**

看起来很强大，对吧？但就更高级的切片而言，这还不是全部。在下一节中，我将介绍动态切片，我们也将自己构建一个。

![device-2018-10-25-102632_1](img/ffab6021cf157998c4fd9aa804befe9a.png)

创建动态切片

首先，我们需要知道交互切片和动态切片的区别。交互式切片用于显示带有动作的静态数据。这意味着切片上显示的所有信息在创建时都是可用的。动态切片用于呈现频繁变化的内容。这种数据可以从应用程序中获取，在 Slice 中修改，然后返回给应用程序。

## 我们将构建简单的动态切片，它将从我们的应用程序中获取值，并能够修改它。当然，相同的值可以通过应用程序进行更改，这种更改应该反映在我们的切片中。我们开始吧！

视图和动作

让我们从实现视图和操作开始。我们需要创建一个包含 InputRange、primary action 和 input action 的简单切片。会是这样的:

## 我们的输入动作是常规的挂起内容，没有任何动作或额外动作。我们可以像这样创建它:

```
private fun createDynamicSlice(context: Context, sliceUri: Uri): Slice {
    return list(context, sliceUri, ListBuilder.INFINITY) {
  inputRange {
  title = "Change value"
  value = 50
  primaryAction = createActivityAction()
            inputAction = createInputPendingIntent()
        }
 } }
```

您可能还记得上一节，我们的意图将由我们的广播接收机所需的所有信息来填充。下一步当然是在我们的应用程序中实现改变值的逻辑。当然也是由广播接收机来完成:

```
val intent = Intent(context, MyBroadcastReceiver::class.java)
return PendingIntent.getBroadcast(context, 0, intent, 0)
```

如你所见，我们再次使用来自 Slice 类的常量来获得额外的值——本例中是我们的范围输入。这就是我们所需要的。为了简洁起见，我将省略将值保存到共享首选项中的整个过程，但是我相信您知道如何处理这一点。

综上所述，现在我们有了一个可以改变我们的值的切片和一个可以接收新值并将其存储在共享偏好中的广播接收器。通过存储在那里，我们可以很容易地在我们的应用程序中使用这个值。因此，让我们为我们的应用程序实现一些真正基本的逻辑，让用户有可能看到当前值并更改它。

```
if (intent.hasExtra(Slice.EXTRA_RANGE_VALUE)) {
    val value = intent.getIntExtra(Slice.EXTRA_RANGE_VALUE, -1)
    if (value != -1) {
        SharedPreferencesUtil.save(context, value)
    }
}
```

应用中的处理值

在我们的一个活动中，我们将需要 TextView 来显示当前值，并需要两个按钮来增加和减少它。每个值的变化都应该写入共享的首选项，因为我们的片将从那里读取它。它的示例代码如下:

## 所以现在我们可以从两个地方修改我们的值:切片和应用。我们需要做的最后一件事是从我们的切片中的共享首选项获取当前值，以确保每当用户更改应用程序中的值时，我们的切片都会随之更新。

动态内容

```
override fun onCreate(savedInstanceState: Bundle?) {
    // ...
    // rest of onCreate body

    minusBtn.setOnClickListener {
  saveValue(getValue() - 1)
        updateView()
    }    plusBtn.setOnClickListener {
  saveValue(getValue() + 1)
        updateView()
    } }

override fun onResume() {
    super.onResume()
    updateView()
}

private fun saveValue(value: Int) {
    SharedPreferencesUtil.save(this, value)
}

private fun getValue(): Int {
    return SharedPreferencesUtil.getValue(this)
}

private fun updateView() {
    valueTv.text = SharedPreferencesUtil.getValue(this).toString()
}
```

要做到这一点，我们必须知道一件事。 **当我们的切片被创建时，我们的应用程序处于严格模式。** 这意味着我们不能执行任何长时间运行的操作，如网络请求或从文件中读取，所以从共享偏好设置中读取值也是不允许的。我们将需要使它异步，并在数据准备就绪时更新我们的内容。这就是动态切片的精髓。

## 对于异步操作，为了清楚起见，我们将使用 Executors 类。当然，您可以在这里使用任何您想要的解决方案，只要它将在后台执行。我们还将创建切片的占位符版本，以指示我们的内容正在被加载——同样，为了简单起见，我们将只使用带有标题的简单行。

同样，这里没有什么真正困难的。创建切片时，我们从共享首选项中触发异步取值。接下来，我们必须检查我们的值是否至少为 0——这意味着数据已被加载。如果我们已经准备好我们的内容，我们将创建我们的切片或简单的占位符否则。但是在 Slice 中更新内容呢？当然会在 fetchcurrentvalueeasync():中完成

```
private var currentValue = -1   private fun createDynamicSlice(context: Context, sliceUri: Uri): Slice {
    fetchCurrentValueAsync()

    return if (currentValue >= 0) {
        list(context, sliceUri, ListBuilder.INFINITY) {
  inputRange {
  title = "Change value"
  value = currentValue
                primaryAction = createActivityAction()
                inputAction = createInputPendingIntent()
            }
 }  } else {
        list(context, sliceUri, ListBuilder.INFINITY) {
  row {
  title = "Loading"
  primaryAction = createActivityAction()
            }
 }    }
}
```

如你所见，在获取新值后，我们再次使用 notifyChange 来让 Slice 托管应用程序更新我们的 Slice 中的内容。

```
private fun fetchCurrentValueAsync() {
    Executors.newSingleThreadExecutor().execute {
  val context = context ?: return@execute    val value = SharedPreferencesUtil.getValue(context)
        if (value != currentValue) {
            currentValue = value
            context.contentResolver.notifyChange(DYNAMIC_SLICE_URI, null)
        }
    } }
```

就这样，我们完成了动态切片。创建它并不难——这是一个非常简单的例子，但是想象一下你的切片可以有多高级！让我们看看最终的效果是什么样的:

很好，不是吗？

最后的话

我们创建了两个简单但功能齐全的切片。整个过程并不难，但我们需要记住几件事:

![ezgif-1-f4a756dae3de (1)](img/5de8c1d76acb1c629e18b90e49ff79f5.png)

Slice 通过广播接收器与应用程序通信

创建切片时，我们的应用程序处于严格模式，因此我们不能进行任何长时间运行的操作

使用 SliceActions 静态方法创建预定义的动作

## 使用切片静态字段从动作意图中检索额外内容

有了这些技巧，你就能创作出丰富、迷人和美丽的切片。它将为您提供许多显示远程内容的新机会。真的值得自己去看一看，自己实验一下。

*   如果您想查看本文中使用的全部代码，请查看 GitHub 资源库:
*   [https://github . com/GabrielSamojlo/Android-slices/tree/advanced](http://web.archive.org/web/20220924171136/https://github.com/GabrielSamojlo/android-slices/tree/advanced)
*   **编码快乐！**
*   Use Slice static fields to retrieve extras from action intents

With those tips in your mind, you will be able to create rich, engaging and beautiful Slices. It will give you many new opportunities to display remote content. It's really worth to take a look and experiment by yourself.

If you want to see the whole code used in this article, check out this GitHub repository:

[https://github.com/GabrielSamojlo/android-slices/tree/advanced](http://web.archive.org/web/20220924171136/https://github.com/GabrielSamojlo/android-slices/tree/advanced)

**Happy coding!**