# Android 中的飞利浦 Hue

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/philips-hue-in-android>

 在过去的几年里，我们已经能够观察到物联网市场的增长。目前，一系列商店提供许多“智能”产品，从冰箱、空气净化器到水壶。

许多采用“智能”技术的物品似乎是个坏主意，但我想提出一个不太新的发明，在我看来，它在不久的将来可能会成为普通的东西。本发明是智能照明系统。

飞利浦 Hue 是一组可以无线控制的变色 LED 灯和传感器。两种通信协议用于控制灯泡:Wifi/以太网和 ZigBee。

下图显示了设备间通信的简图:

![image1-22](img/67b8e06e1289e6008cac7f451bfd9aaa.png)

这个过程是这样进行的:

*   您想要用来控制智能照明的设备。它可以是装有 Android 或 iOS 的智能手机，也可以是桌面应用程序
*   在应用程序和网桥之间转发数据的路由器。
*   网桥，其作用是使用 ZigBee 通信协议向灯泡传递信息。
*   接收器-带 ZigBee 模块的灯泡。

Zigbee 是一种高级通信协议，用于创建个人局域网，以实现家庭自动化等目的。它最大的优点是耗电量低。关于这项技术的更多详细信息可以在[这里](/web/20221004141204/https://www.netguru.com/blog/the-zigbee-protocol)找到。

最近决定学点这个话题的东西，选择落在了 Android 平台上。在这篇文章中，我想提出我的想法和使用的例子。

通常，为了让我们的应用程序开始控制灯泡或传感器，我们必须引导用户完成一个简短的配置过程，包括以下步骤:

1.显示局域网内可用网桥的列表，并要求用户选择一个。

2.开始授权过程，并要求用户按下网桥顶部的大圆形按钮。

3.将网桥的本地地址和生成的登录信息保存在手机存储器中。

为了实现第一点，我们必须创建一个“PHHueSDK”的实例，它可以由像 Dagger2:

```
phHueSDK = PHHueSDK.create().apply {
	appName = "AppName"
	deviceName = android.os.Build.MODEL
}
```

然后我们需要设置一个监听器:

```
phHueSDK.notificationManager.registerSDKListener(this)
```

开始搜索:

```
val searchManager = phHueSDK.getSDKService(PHHueSDK.SEARCH_BRIDGE) as PHBridgeSearchManager
searchManager.search(true, false)
```

“search()”中的第一个参数表示我们正在使用“UPnP”搜索桥。

调用 search 方法后，我们现在可以用“PHSDKListener”来监听事件。

```
public interface PHSDKListener {
	void onCacheUpdated(List<Integer> cacheNotificationsList, PHBridge bridge);
	void onBridgeConnected(PHBridge bridge, String userName);
	void onAuthenticationRequired(PHAccessPoint accessPoint);
	void onAccessPointsFound(List<PHAccessPoint> accessPoints);
	void onError(int code, String message);
	void onConnectionResumed(PHBridge bridge);
	void onConnectionLost(PHAccessPoint accessPoint);
	void onParsingErrors(List<PHHueParsingError> parsingErrors);
} 
```

检测到的桥将被传递给“onAccessPointsFound()”函数，在这里我们可以更新显示给用户的列表:

```
override fun onAccessPointsFound(accessPoints: MutableList?) {
	// (...)
    view.refreshAccessPoints(accessPoints)
}
```

用户按下按钮后，我们应该连接到选定的网桥:

```
override fun onBridgeClicked(bridge: PHAccessPoint) {
    val selectedBridge = hueSdk.selectedBridge
    selectedBridge?.let { bridge ->
        val connectedIP = bridge.resourceCache.bridgeConfiguration.ipAddress
        if (connectedIP != null) { // since we might be already connected to another bridge
            hueSdk.disableHeartbeat(bridge)
            hueSdk.disconnect(bridge)
        }
    }
    hueSdk.connect(bridge)
}
```

如果选定的桥需要验证，将调用“onAuthenticationRequired()”函数:

```
override fun onAuthenticationRequired(accessPoint: PHAccessPoint?) {
    hueSdk.startPushlinkAuthentication(accessPoint)
    view.showPressButtonMessage()
}
```

在这种情况下，我们应该调用“huesdk . startpushlinkauthentic ation”函数，并要求用户按下他的网桥顶部的按钮，以确认来自应用程序的访问。

如果一切顺利，将调用‘onbridgeconnected’函数。这是我们应该保存网桥的 IP 地址和用户名以便将来授权的地方:

```
override fun onBridgeConnected(bridge: PHBridge, username: String) {
    val ipAddress = bridge.resourceCache.bridgeConfiguration.ipAddress
    bridgePrefs.ipAddress = ipAddress
    bridgePrefs.userName = username
    view.showSuccess()
}
```

在成功的授权过程之后，我们能够使用先前保存的数据连接到网桥:

```
fun connect() {
    val lastAccessPoint = PHAccessPoint()
    lastAccessPoint.ipAddress = userPrefs.ipAddress
    lastAccessPoint.username = userPrefs.userName

    if (!hueSdk.isAccessPointConnected(lastAccessPoint)) {
        hueSdk.connect(lastAccessPoint)
    }
}
```

现在我们可以开始控制色调设备了。使用“PHHueSDK”对象，我们能够获得诸如灯光、房间、桥梁配置、规则、场景、时间表、传感器等数据。例如，让我们打开连接到桥上的所有灯:

```
private fun getBridge() = hueSdk.selectedBridge

private fun getLights(bridge: PHBridge) = bridge.resourceCache.allLights

fun enableLights(enabled: Boolean) {
    val bridge = hueSdk.selectedBridge // get connected bridge
    val allLights = bridge.resourceCache.allLights // get all lights
    allLights.forEach { light -> 
        val lightState = PHLightState() // create new state
        lightState.isOn = enabled // set state to enabled
        bridge.updateLightState(light, lightState, this) // update the state of bulb
    }
} 
```

“这是一个“PHLightListener ”,我们可以在其中监听事件，如“onSuccess()”,当灯的状态成功改变时，或“on error”，用错误代码处理它:

```
void onSuccess();
void onReceivingLightDetails(PHLight light);
void onSearchComplete();
void onStateUpdate( Map<string, string=""> successAttribute, List errorAttribute);
void onReceivingLights(List lights);
void onError(int code, String errorMessage);
```

就这样，我们经历了控制智能灯泡这个最简单的案例。就个人而言，我认为智能照明的想法很有可能在未来几年成为一种标准，只要它们的价格变得更加实惠。

以下是包含更多有趣信息的一些来源:

* * *

照片由 [罗汉·马卡查](http://web.archive.org/web/20221004141204/https://unsplash.com/photos/jw3GOzxiSkw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [下](http://web.archive.org/web/20221004141204/https://unsplash.com/search/photos/light?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)