# 你好即时应用

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/hello-instant-apps>

 [即时应用是向用户介绍本地应用体验的绝佳方式，无需安装](/web/20220927121741/https://www.netguru.com/blog/android-instant-apps)。由于其大小限制(4MB)，称为功能的应用程序片段可以快速下载。这为那些负担不起安装整个应用程序或不想为一个功能下载整个应用程序的人带来了出色的用户体验。只需将应用程序链接分配到您的功能模块。现在，每当用户点击即时应用程序模式中的链接，并且您的应用程序通过验证，而不是打开浏览器，Play Store 将下载您的功能，缓存它并启动您的本地应用程序的一部分。

## 要求和限制

即时应用带来了一些新的功能、要求和限制。

##### **即时应用需求:**

在开始开发你的第一个即时应用程序之前，你需要满足几个要求:

##### **即时应用程序限制:**

正如我之前提到的，即时应用有一些限制。您需要记住，与 API 的通信必须通过 https 来完成。你的应用将无法使用后台服务、发送后台通知或访问唯一的设备标识符。

即时应用的权限列表也减少了，这里是全部:

## 创建你的第一个即时应用

从头开始构建即时应用非常简单。但是在你开始之前，我建议你花一些时间考虑一下你的项目应该是什么样子，它会有什么特性，它会共享什么代码库/资源/依赖。以后会派上用场的。现在让我们开始创建我们的第一个即时应用程序。

## ![select_the_form_factors_and_minimum_sdk](img/6243fe1bb89cb505cb81fc1306d75bfc.png)

创建新项目时，记得选择 API 21 或更高版本，同时确保选中“*包括 Android 即时应用支持*”复选框。

![configure_feature](img/c5b9fa1356b99d96de9dbe97b3c47523.png)

您现在可以指定您的第一个功能模块名称。Android Studio 将创建模块

*   *app*——负责生成可安装的 apk

*   *基础*——所有特性之间共享的模块

*   *instant app*——生成包含所有功能模块的即时 app zip 包 *apks*

**

 *向功能模块添加新活动时，系统会要求您指定应用程序链接。意图过滤器将仅使用 https 方案创建，因此我建议您转到清单文件并包含 http。

*![new_empty_activity](img/bb5fae0d3658a761feabf49fbeeb00f5.png)**

*在最后一步之后，您应该已经生成了四个模块。现在让我们更仔细地看一看它们。*

 ***app(gradle plugin:apply plugin:' com . Android . application ')**

其主要目的是将所有模块合并成可安装的 apk。如果你的可安装应用程序与即时应用程序没有区别，这个模块将保持原样。只要记住添加每一个新功能作为依赖。

```
apply plugin: 'com.android.application'

android {
   ...

   defaultConfig {
       applicationId "com.example.android.myapplication.app"
       ...
   }

   ...

}

dependencies {
   implementation project(':feature')
   implementation project(':base')
}
```

**instant app(gradle plugin:apply plugin:' com . Android . instant app ')**

只需在最终的 instant app zip 文件中添加您想要的所有功能的依赖关系。你也可以在这里添加建筑类型和风格。

```
apply plugin: 'com.android.instantapp'

dependencies {
   implementation project(':feature')
   implementation project(':base')
}
```

**base(gradle plugin:apply plugin:' com . Android . feature ')**

这里我们有一些新的东西: *baseFeature true。* 这个设置告诉 *gradle* 所有的 rest 特性都将从这个特性中获得它们共享的代码和资源。由于基本特性 应用程序项目(':app') 声明，我们不需要为 instantapp 模块指定应用程序 id。一个基地将从我们的可安装模块中获取它所需要的一切。请记住，这里还包括所有功能模块。在向基本模块添加代码、依赖项和资源时，请记住，无论是基本模块还是功能模块，都不能超过为当今即时应用程序设置的 4MB 限制。

```
apply plugin: 'com.android.feature'

android {
   compileSdkVersion 28
   baseFeature true
   defaultConfig {
       minSdkVersion 21
       targetSdkVersion 28
       versionCode 1
       versionName "1.0"
   }
   ...
}

dependencies {
   api 'com.android.support:appcompat-v7:28.0.0-rc01'
   api 'com.android.support.constraint:constraint-layout:1.1.2'
   application project(':app')
   feature project(':feature')
}
```

**功能(gradle 插件:应用插件:' com.android.feature')**

正如你所看到的，一个功能模块不同于你的可安装模块，因为它没有实现所有的功能，而且我们不需要指定应用程序 id，因为它有一个基本的功能依赖关系。

##### 这是你的第一个 hello world 即时应用程序。现在通过文件/新建/新建模块/特性模块添加一些新特性

```
apply plugin: 'com.android.feature'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'

android {
   ...
}

dependencies {
   implementation fileTree(dir: 'libs', include: ['*.jar'])
   implementation"org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
   implementation project(':base')
   ...
}
```

请记住，从现在开始，你需要像这样开始你的活动:

**应用链接变得简单(工具/应用链接助手)**

App Links Assistant 是管理你的 App 链接的有用工具。让我提一下它的一些特性

```
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://android.example.com/character")
startActivity(intent)
```

### **URL 映射**

要访问此功能，您需要打开 URL 映射编辑器，并在 Url 映射表中按“+”。使用这个工具，您可以轻松地将您的活动映射到您想要的链接。如果你想使用这个工具，你必须记住把不同的 *<数据/ >* 放在不同的 *<意图过滤>* 块中，否则这个工具在区分每个 URL 映射时会有一些问题。重新格式化选项真的很差，所以我建议手动操作，或者干脆不要使用这个工具。

##### 编辑后，适当的过滤器将被添加到 *AndroidManifest 文件*

##### 记住总是用 http 方案复制这个过滤器

**关联网站**

```
<intent-filter>  <action android:name="android.intent.action.VIEW" />    <category android:name="android.intent.category.DEFAULT" />  <category android:name="android.intent.category.BROWSABLE" />    <data  android:scheme="https"  android:host="android.example.com"  android:pathPattern="/character" />  </intent-filter>
```

为了能够将您的即时应用程序 zip 包上传到 Play Store，您的应用程序链接需要经过验证。为此，您需要将数字资产链接 json 文件添加到您网站的已知位置 *({site_domain}/)。【T2 知名/assetlinks.json】。有了这个工具，你所要做的就是指定你的应用程序 id 和网站域，如果你想使用智能锁 API，你应该检查支持共享凭证...如果与域名相同，则添加登录 URL 或标记复选框。点击 generate 按钮，您就可以下载/复制并粘贴 assetlinks.json 文件了。*  更多信息:[https://developer . Android . com/training/app-links/verify-site-associations](http://web.archive.org/web/20220927121741/https://developer.android.com/training/app-links/verify-site-associations)

**运行** **你的即时 App**

##### 有几种方法可以运行即时应用程序。

### **定制版本**

 **如您所见，您需要将 启动 从 默认活动 更改为 URL ，并指定您想要的 URL。别担心，Android Studio 不会允许你用错误的 URL 创建配置，因为它会在接受之前扫描你的 AndroidManifest 文件。

**当您的即时应用程序在设备上运行时，启动 ADB 的功能**

**![custom_build](img/045bc807fb0047bde06fa0f6182a9d1a.png)**

如果你想让启动 一个特定的特性，你可以使用这个 ADB 命令:

**测试应用链接**

转到应用链接助手，并选择最后一个选项测试应用链接。从这里你可以选择你的应用程序应该从哪个模块运行安装/即时应用程序，并添加所需的网址。如果 Android Studio 找不到与 URL 相关联的<inetnt-filter>，您将无法启动 run。</inetnt-filter>

**立即尝试按钮**

```
adb shell am start -a android.intent.action.VIEW -d “{url}”
```

##### 在成功将应用程序链接添加到您的所有活动后，现在是时候决定哪一个将成为您的即时应用程序默认活动，哪一个将由**立即尝试** Play Store 的按钮启动。为此，您需要将 *<元数据/ >* 块添加到活动中

**关于空间要求的更多信息**

![test_on_device](img/e6ea2a871484e9d549e2e0fefd63401f.png)

### 使用网页浏览图片

删除未使用的代码

```
 <meta-data  android:name="default-url"  android:value="https://android.example.com/character" /> 
```

尝试为即时应用程序找到轻量级的库，并使用继承和合成将它们替换为已安装版本中所需的库

使用重构/移除未使用的资源...

We already know how important is the size of your features so I would like to share some great tips for reducing your apks sizes:  

*   使用 gradle 中的 splits 将资源拆分成不同的 apk
*   正如我之前告诉你的，即时应用基本功能和任何其他功能加起来不能超过 4 MB 的大小限制。并不意味着你的整个 app 不能像你想的那么大。你所要记住的就是坚持 4 MB 规则。例如，如果您的基本功能大小为 2 MB，则所有功能占用的空间必须小于 2MB。将即时应用程序上传到 Play Store 时，将检查您的功能大小。
*   谷歌最近推出了针对即时应用的 10 MB 计划。如果你想参加，你所要做的就是填写这张[表格](http://web.archive.org/web/20220927121741/https://docs.google.com/forms/d/e/1FAIpQLScVKwTCzjeh6brMyCKpJqdcxZXW7WX9ho__TCj5YVEz-Kaovw/viewform)。
*   **在即时应用中处理程序**
*   使用即时应用程序，你可能至少需要两个 proguard 规则文件。一个用于应用程序的可安装版本，另一个用于基本功能模块。首先，为你的可安装版本创建规则，并用 apply 插件将它添加到一个模块中:' *com.android.application* '。现在在特性中，proguard 变得有点棘手，因为你的应用程序不会被构建到一个大的 apk 文件中。每个模块都将单独构建，然后打包成一个 zip 文件。所以你的基本特性不知道你的一个特性使用了它的一个 util 类，你需要告诉 proguard 把这些类分开。apkanalzyer 的少量文字处理和使用将对此有用。使用任何具有的变体构建您的应用

```
android {
    ...
    splits {
        generatePureSplits true
        density {enable true }
    }
} 
```

*minifyEnabled* 设为 *false* 。现在，对于位于构建文件夹中的所有特性 apk，运行命令:

### 该命令行将生成一个类列表，您必须将这些类作为 keep 规则添加到您的功能模块的 proguard 文件中。

更多信息  [https://medium . com/Google-developers/enabling-proguard-in-an-Android-instant-app-FBD 4 fc 014518](http://web.archive.org/web/20220927121741/https://medium.com/google-developers/enabling-proguard-in-an-android-instant-app-fbd4fc014518)

**一些通用提示**

```
$ comm -23 <(~/Library/Android/sdk/tools/bin/apkanalyzer dex packages detail-debug.apk | grep "^C r" | cut -f4 | sort) <(jar tf ~/Android/Sdk/platforms/android-xx/android.jar | sed s/.class$// | sed -e s-/-.-g | sort)
```

即使您在创建项目时选择了 kotlin 支持，基本功能模块也不会自动添加 koltin 依赖项和插件

在即时应用和已安装应用之间切换测试之前，记得从设备上移除应用，否则会导致运行时崩溃

尽管权限严格，您仍然可以访问内部存储器

### 即时应用程序可以访问[Cookie API](http://web.archive.org/web/20220927121741/https://developer.android.com/reference/android/content/pm/PackageManager.html#getInstantAppCookie())所以使用它将一些用户数据从即时应用程序转移到已安装的应用程序是明智的。  为了保证每个应用从你的 URL 启动一个即时 app 使用 [Firebase 动态链接](http://web.archive.org/web/20220927121741/https://firebase.google.com/docs/dynamic-links/):  Google 推荐的用户授权方式，是使用 [AUTH 服务](http://web.archive.org/web/20220927121741/https://developers.google.com/identity/smartlock-passwords/android/overview) 配合 [即时 App API](http://web.archive.org/web/20220927121741/https://developers.google.com/android/reference/com/google/android/gms/instantapps/package-summary)

*   [package manager compat](http://web.archive.org/web/20220927121741/https://developers.google.com/android/reference/com/google/android/gms/instantapps/PackageManagerCompat.html)——允许您在 API < 26 的设备上使用 cookie API
*   [Instantapps . showinstallprompt](http://web.archive.org/web/20220927121741/https://developers.google.com/android/reference/com/google/android/gms/instantapps/InstantApps.html#showInstallPrompt(android.app.Activity,%20android.content.Intent,%20int,%20java.lang.String))-在您的应用中显示完整的应用版本安装对话框，建议至少有一个绑定安装提示的按钮
*   从 PackageManager 中， [isInstantApp()](http://web.archive.org/web/20220927121741/https://developers.google.com/android/reference/com/google/android/gms/instantapps/PackageManagerCompat.html#isInstantApp()) 函数 告诉你用户使用的是即时应用还是完全安装的应用。在显示安装提示之前调用它
*   **总结**

*   总的来说，我认为即时应用会给你的用户和团队带来好处。从用户的角度来看，您无需安装即可获得快速的原生体验。用户可以通过朋友发给他们的链接直接进入你的应用。至于你的团队，你拥有模块化项目的所有好处，易于重构或完全改变各自模块中的特性。有了计划良好的结构，项目维护也将变得容易。
*   也有一些缺点。您需要记住，您的权限可用性是有限的。因此，如果你的功能使用 NFC，你可以忘记将它包括在你的即时应用程序中。一些后台进程受到限制。不要忘记大小限制，仅支持重量为 2MB 的 libs，因此如果没有 Proguard，您将无法发布即时应用程序。由于构建即时应用程序的方式，Zip Bundle Proguard 不像可安装的 apk 那样工作，你需要付出更多的努力来解决它。总的来说，我强烈推荐尝试即时应用程序。
*   From the PackageManager, the [isInstantApp()](http://web.archive.org/web/20220927121741/https://developers.google.com/android/reference/com/google/android/gms/instantapps/PackageManagerCompat.html#isInstantApp()) function tells you if a user uses an application as Instant App or fully installed. Call it before showing the install prompt

## **Summary**

Overall I think Instant Apps will bring benefits to your users and your team. From a user perspective, you get a fast native experience without the need of installation. Users will be able to jump right into your app from links sent to them by friends. As for your team, you have all the benefits from modularizing your project, easy to refactor or completely change features each in its own module. Project maintenance will also be easy with well-planned structure.

There are also some disadvantages. You need to remember that you have limited permission usability. So, if your feature uses NFC you can forget about including it to your Instant App. Some of the background processes are limited. Don't forget about size limitations, support libs weight 2MB alone, so without Proguard, you won't be able to ship your Instant App. Thanks to the way of building Instant Apps, Zip Bundle Proguard doesn't work like in an installable apk and you need to make more effort to work it out. Overall I would highly recommend trying out instant apps.***