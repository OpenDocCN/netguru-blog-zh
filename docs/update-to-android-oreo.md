# 将您的应用程序更新到 Android Oreo——应用程序所有者的重要信息

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/update-to-android-oreo>

 谷歌开始发送电子邮件通知谷歌 Play 商店政策即将发生的变化。从 2018 年 11 月 1 日开始，每一个作为新应用或之前发布的应用的更新上传的应用都必须针对[安卓](/web/20221001234426/https://www.netguru.com/services/android-mobile-development)奥利奥(API 等级 26)或更高。

## 我为什么要在乎？

### 后台执行限制

[安卓](/web/20221001234426/https://www.netguru.com/services/android-mobile-development) O 通过改变后台服务的行为，持续提升电池续航能力。虽然默认情况下，新的限制不适用于旧的应用程序，但用户可以从设置中手动强制新的行为，这可能会导致旧应用程序出现问题。谷歌为移动开发者提供了可能的替代品，比如 JobScheduler。

### 平台版本市场份额

#### ![image1-16](img/709a62f2412f1e9b01c979ad6cb8abe3.png)

*来源:【https://developer.android.com/about/dashboards/】*

Android 8.0+的市场份额在不断增长，所以当你瞄准较低的 API 时，你已经让 14.6%的潜在用户面临糟糕的用户体验，这可能导致不利的 Play Store 评论。

### 安卓后台位置限制

为了节省电池，并保持相同水平的用户体验和系统健康，在运行 Android 8.0 的设备上使用时，后台应用程序接收位置更新的频率会降低。如果您的应用程序依赖实时警报或后台运行的运动检测，记住这种位置检索行为尤为重要。

### App 快捷方式

com . Android . launcher . action . install _ SHORTCUT 广播不再对您的应用程序有任何影响，因为它现在是一个私有的隐式广播。相反，您应该通过使用 ShortcutManager 类中的 requestPinShortcut()方法来创建应用程序快捷方式。

### 通知

奥利奥对通知进行了修改。您的应用程序可能不会显示与用户交互所需的重要通知。

### 总体改进

除了不好的方面，新的 API 在安全性和用户隐私方面做了很多改进。Android 8.1 (API level 27)对自动填充框架进行了一些改进，你可以将这些改进整合到你的应用中。通过使用安全浏览 API 的 WebView 实现，您的应用程序可以检测 WebView 实例何时试图导航到 Google 已归类为已知威胁的 URL。Android 支持库 26 允许您从提供商应用程序请求字体，而不是将字体捆绑到 APK 或让 APK 下载字体。新版本的 Android 还为创建自己的辅助功能服务的开发人员增加了对几个新的辅助功能的支持。

## 最终想法

将最新 Android 设备的用户排除在应用程序的预期工作流程之外，肯定会给你带来一些差评。迁移您的应用程序以支持 API 26+将消耗您的一些时间，但从长远来看，API 的常规修复和改进带来的好处(新功能、与用户交互的新方式，以及最重要的是不会让用户面临 API 限制导致的不可预测的行为)肯定会带来回报。

一些有用的资源: