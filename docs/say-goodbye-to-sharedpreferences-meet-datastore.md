# 告别 SharedPreferences - Meet 数据存储

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/say-goodbye-to-sharedpreferences-meet-datastore>

 前段时间，谷歌推出了 Android Jetpack 的一个新部分，DataStore。这是一个应该取代 SharedPreferences 的库。这就是这个吸引人的标题的原因:迟早，我们所有人都可能被迫转向数据存储。

这两个 API 之间有一些相似之处，但是 DataStore 提供了更多的灵活性。它有两种不同的储物方式。第一个是简单的键值对存储，就像 SharedPreferences 一样。第二种类型是 Proto DataStore，更有趣也更复杂。它基于 Google 的 Protobuf 库，允许我们创建更复杂的数据结构。整个数据存储目前在 alpha 中。在深入讨论细节之前，让我们先来看一下 DataStore 和 SharedPreferences 之间的比较。

## 共享首选项和数据存储比较

*   DataStore 构建在 Kotlin 协同例程和流之上，因此它为修改和读取数据提供了一个非常好的异步 API。另一方面，SharedPreferences 只通过一个侦听器来提供异步访问。
*   可以在 UI 线程上调用 DataStore，没有任何问题。它将调度程序改为引擎盖下的 IO。
*   数据存储在数据解析期间也不会出现运行时异常。

值得一提的是，数据存储不是空间的替代品。它只适用于小型数据集以及不需要部分更新或参照完整性的情况。如果你需要这些，考虑使用房间。

## 数据存储的类型

如前所述，我们有两种类型的数据存储可用:

*   Preferences DataStore，它与 SharedPreferences 非常相似。它存储键值对，不提供任何类型安全。
*   更复杂的数据存储原型。它可以在自定义对象中存储数据。由于这一点，它提供了开箱即用的类型安全。在设置过程中需要做更多的工作，因为需要使用 [协议缓冲区](http://web.archive.org/web/20221006050453/https://developers.google.com/protocol-buffers) 创建一个模式。

## 偏好数据存储

首先，让我们看看键值存储。设置非常简单。要开始使用这个版本的 DataStore，只需向应用程序的 build.gradle 添加一个依赖项。

```
// Preferences DataStore
implementation "androidx.datastore:datastore-preferences:1.0.0-alpha02"
```

我创建了一个简单的 ProfilePreferences 类来处理数据存储。基本上，Preferences 中只存储一个简单的布尔值。它确定是否可以更改配置文件信息。

```
 class ProfilePreferences(context: Context) {

	private val dataStore: DataStore<Preferences> = context.createDataStore(
    	"profile",
    	migrations = listOf(SharedPreferencesMigration(context, "oldProfilePreferences"))
    )
	val editStateFlow: Flow<Boolean> = dataStore.data
    	.map{ it[EDIT_MODE_ENABLED_KEY] ?: true }

	suspend fun toggleEditMode(enabled: Boolean) {
    	dataStore.edit {
        	it[EDIT_MODE_ENABLED_KEY] = enabled
       	}
 	}

	companion object {
		private val EDIT_MODE_ENABLED_KEY = preferencesKey<Boolean>("edit_mode_enabled")
  	}
}
```

*   ProfilePreferences 需要上下文来创建数据存储。有了这些信息，我们可以使用扩展函数`createDataStore()`并传递数据存储的名称。
*   可以从 SharedPreferences 添加迁移。只需传递一个带有要迁移的首选项名称的 SharedPreferenceMigration 对象。
*   DataStore 不使用普通字符串作为键，它们由 Key a 类包装。要获得特定的密钥，需要使用`preferencesKey<T>(name)`。Type T 是存储在此项下的值的所需类型。
*   要更改存储的值，我们可以使用 DataStore 对象中的`edit()`函数。在 lambda 中，我们可以访问 MutablePreferences，因此我们可以更改指定键下的值。`edit()`函数是一个挂起函数，因此需要从 CoroutinesContext 中调用。
*   为了获得存储的偏好，我们可以访问`dataStore.data`下的`Flow<Preferences>`。使用`map{}`操作符，我们可以得到`Flow<Boolean>`。

要更改 EDIT_MODE 的值，只需使用 ViewModel 中`ProfilePreferences`的`toggleEditMode()`方法。

```
 fun toggleEditMode(enabled: Boolean) {
	viewModelScope.launch {
		profileRepository.toggleEditMode(enabled)
	}
} 
```

当该操作完成时，使用 Flow 共享一个新值。在我们的例子中，我使用`asLiveData()`扩展将该流转换成 LiveData，并且在活动中观察到了这一点。

```
 val editEnabled = profileRepository.editStateFlow.asLiveData()

viewModel.editEnabled.observe(this) {
	with(binding) {
		name.isEnabled = it
		surname.isEnabled = it
	}
} 
```

很容易使用，对吧？现在让我们跳到原型数据存储示例。

## 原型数据存储

正如我之前提到的，Proto DataStore 可以存储定制数据的实例。为此，您必须使用 [协议缓冲区](http://web.archive.org/web/20221006050453/https://developers.google.com/protocol-buffers) 来定义该数据的模式。我的项目中使用的示例模式如下所示。

```
 syntax = "proto3";
option java_package = "com.netguru.datastoresample";
option java_multiple_files = true;

message ProfileInfo {
	string surname = 1;
	string name = 2;
} 
```

如您所见，除了数据结构，还有其他选项。默认情况下，缓冲区使用`proto2`,所以为了使用最新的语法，我们必须显式地提供它。另外两个选项定义了应该在哪里以及如何创建生成的 Java 类。这个文件中最后也是最重要的是消息定义。这是编译器应该如何生成新类的模式。

当我写这篇文章时，Proto DataStore 的 [文档](http://web.archive.org/web/20221006050453/https://developer.android.com/topic/libraries/architecture/datastore#proto-datastore) 缺少一些关于配置部分的细节。基于这些文档，有人会认为这就足够了。不幸的是，情况并非如此，我发现要正确设置一切有点困难。缺少关于基于定义的模式生成 Java 类的部分。

我选择了 [线](http://web.archive.org/web/20221006050453/https://github.com/square/wire) 进行代码生成。也有一些官方的 Google 工具，但是 Wire 可以生成 Kotlin 类。它是专门为 Android 设计的，因此优化得更好，生成的代码也更干净。Google tools 生成的同一个类有大约 400 行代码，而 Wire 只生成了 130 行。Wire 生成的类也是可打包的，所以我们可以简单地通过一个包来传递它们。说到配置，我们需要几样东西:

*   插件，你应该添加到你的根版本中。

```
classpath "com.squareup.wire:wire-gradle-plugin:3.3.0"
```

你还需要在你的应用程序的 build.gradle 中添加这个插件。

```
apply plugin: 'com.squareup.wire'
```

*   接下来，您需要添加 Protobuf 运行时库和编译器。

```
implementation "com.squareup.wire:wire-runtime:3.3.0"

implementation "androidx.datastore:datastore-core:1.0.0-alpha02" 
```

最后但同样重要的是，为 Gradle 插件添加配置块。

```
 wire {
  kotlin {	
    android = true
  }
} 
```

如果您已经准备好了模式，只需重新构建项目，一切都将正确生成。

让我们回到模式示例。

```
 message ProfileInfo {
	string surname = 1;
	string name = 2;
} 
```

Protobuffers 支持几种数据类型。首先，它们可以存储所有标量，如整数、双精度数等。我们也可以使用枚举甚至其他消息作为类型。更多信息，可以查看 [文档。](http://web.archive.org/web/20221006050453/https://developers.google.com/protocol-buffers/docs/overview#simple)

提供的示例模式仅包含 2 个字符串字段。编译器将基于该消息创建一个 ProfileInfo 类。不要将分配给字段的数字与默认值混淆。这些数字是生成器添加到这些值中的标签。

当我们已经有了一个生成的模型类时，我们需要创建一个序列化器来持久化这些数据。

```
 object ProfileInfoSerializer : Serializer<ProfileInfo> {

	override fun readFrom(input: InputStream): ProfileInfo {
		try {
			return ProfileInfo.ADAPTER.decode(input)
		} catch (exception: InvalidProtocolBufferException) {
			throw CorruptionException("Cannot read proto.", exception)
		}
	}

	override fun writeTo(t: ProfileInfo, output: OutputStream) = ProfileInfo.ADAPTER.encode(output,t)
} 
```

它可能看起来像样板代码，而且在大多数情况下可能会是这样。但是，通过这种抽象，我们可以在将数据保存到数据存储之前对其进行加密。

最后，让我们看看数据存储本身的使用和创建。它看起来非常类似于首选项数据存储。

```
 class ProfileStore(context: Context) {

	private val dataStore: DataStore<ProfileInfo> = context.createDataStore(
    	fileName = "basket_item.pb", 
        serializer = ProfileInfoSerializer
   	)

    val profileInfoFlow = dataStore.data

	suspend fun changeName(name: String) {
		dataStore.updateData {
			it.copy(name = name)
		}
	}

	suspend fun changeSurname(surname: String) {
		dataStore.updateData {
 			it.copy(surname = surname)
		}
	}
} 
```

要创建一个原型数据存储，我们需要提供一个存储数据的文件名和我们之前创建的序列化程序。

如果您想从 SharedPreferences 迁移到 Proto DataStore，您可以。您所要做的就是创建一个 map 函数，将 SharedPreferences 中的数据添加到 model 类的特定字段中。

```
 private val dataStore: DataStore<ProfileInfo> = context.createDataStore(
	fileName = "profile_info.pb", 
    serializer = ProfileInfoSerializer,
	migrations = listOf(SharedPreferencesMigration(context,"profile_preferences") 
    { sharedPreferences: SharedPreferencesView, profileInfo: ProfileInfo ->
		// Map preferences into profile info
	})
) 
```

## 摘要

与 SharedPreferences 相比，DataStore 有许多优势。一个是对协程和流的支持，这使得异步读写成为可能，并且可以安全地从 UI 线程调用。然后是对错误处理的支持。在我看来，这将是 SharedPreferences 的一个很好的替代品。迁移选项使它变得更好，因为它可以在任何项目中实现，而不仅仅是在新项目中。我鼓励您尝试一下，即使是在一个示例项目中，并且更加熟悉这个库。

照片由[卢卡斯·范·奥尔特](http://web.archive.org/web/20221006050453/https://unsplash.com/@switch_dtp_fotografie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄于[Unsplash](http://web.archive.org/web/20221006050453/https://unsplash.com/s/photos/storage-container?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)