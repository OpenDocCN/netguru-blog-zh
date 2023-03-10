# Android Jetpack 安全性

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/androidx-security>

 介绍

安全性无疑是移动应用程序中的一个重要元素，但不幸的是，正确地做每件事是一项复杂的任务。最近，谷歌发布了其安全加密库，作为 [jetpack 组件](http://web.archive.org/web/20221205003721/https://developer.android.com/jetpack)的一部分，以简化使应用程序更加安全的过程。

目前，该库包括三个主要功能:

*   万能钥匙
*   EncryptedSharedPreferences
*   加密文件

AndroidX Security 正在使用幕后的 Tink 库。Tink 是 Google 创建的一个开源加密库，提供加密 API。

## 装置

要使用这个库，你只需要将这一行添加到应用程序的 Gradle 文件中:

```
implementation `androidx.security:security-crypto:1.0.0-alpha02`
```

Security-crypto 目前处于 alpha 版本，但你可以在 [mvnrepository 页面](http://web.archive.org/web/20221205003721/https://mvnrepository.com/artifact/androidx.security/security-crypto?repo=google)上监测它的新版本。它的大小真的很小——在分析了它在`11.9 KB`附近添加的编译过的`.apk`文件之后。

为了使用这个库，您需要将`minSdkVersion`设置为`23+`——由于使用了新的 API，Keystore 操作特别稳定(`[KeyPairGeneratorSpec](http://web.archive.org/web/20221205003721/https://developer.android.com/reference/android/security/KeyPairGeneratorSpec)`已经被 API 23 `[KeyGenParameterSpec](http://web.archive.org/web/20221205003721/https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec)`取代)。

## 万能钥匙

`MasterKeys`是一个包含一个公共方法`getOrCreate`的助手类，该方法允许开发者创建一个主密钥，然后为它获得一个别名。让我们分析一下代码:

```
@NonNull
public static String getOrCreate(
        @NonNull KeyGenParameterSpec keyGenParameterSpec)
        throws GeneralSecurityException, IOException {
    validate(keyGenParameterSpec);
    if (!MasterKeys.keyExists(keyGenParameterSpec.getKeystoreAlias())) {
        generateKey(keyGenParameterSpec);
    }
    return keyGenParameterSpec.getKeystoreAlias();
} 
```

唯一的参数`KeyGenParameterSpec`指定了算法、块模式、填充和密钥大小等选项。放置在`MasterKeys.AES256_GCM_SPEC`下的默认值创建以下对象:

```
 @NonNull
private static KeyGenParameterSpec createAES256GCMKeyGenParameterSpec(
        @NonNull String keyAlias) {
    KeyGenParameterSpec.Builder builder = new KeyGenParameterSpec.Builder(
            keyAlias,
            KeyProperties.PURPOSE_ENCRYPT | KeyProperties.PURPOSE_DECRYPT)
            .setBlockModes(KeyProperties.BLOCK_MODE_GCM)
            .setEncryptionPaddings(KeyProperties.ENCRYPTION_PADDING_NONE)
            .setKeySize(KEY_SIZE);
    return builder.build();
} 
```

其中`keyAlias`是`_androidx_security_master_key_`和`KEY_SIZE = 256`。

## EncryptedSharedPreferences

`EncryptedSharedPreferences`是`SharedPreferences`的一个包装类，它允许你保存和读取加密和解密的值。让我们看看如何创建它的一个实例:

```
EncryptedSharedPreferences.create(
    PREFS_FILENAME,
    masterKeyAlias,
    applicationContext,
    EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
    EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
)
```

第二个参数是使用`MasterKeys.getOrCreate(KeyGenParameterSpec)`创建的别名，最后两个参数定义了用于加密密钥和值的方案。目前，唯一可用的`EncryptedSharedPreferences.PrefKeyEncryptionScheme`是`AES256_SIV`，唯一可用的`EncryptedSharedPreferences.PrefValueEncryptionScheme`是`AES256_GCM`。

然后我们能够像使用一个典型的`SharedPreferences`对象一样使用它。下面是保存一个`String`的例子:

```
encryptedPrefs.edit {
    putString(ENC_KEY, value)
    apply()
} 
```

并读取保存的值:

```
val string = encryptedPrefs.getString(ENC_KEY, null) 
```

下面是一个简短的视频，展示了它在我的例子中是如何工作的:

加密文件

![android_security_prefs](img/169a1951f6db33a8dd9c7f740721a92f.png)

`EncryptedFile`允许您使用`FileInputStream`轻松加密数据，使用`FileOutputStream`轻松解密。要创建它的实例，我们需要几样东西:

## 第一个参数是一个`File`对象，它定义了存储加密数据的路径和名称。在我的例子中，我创建了一个这样的实例:

使用`masterKeyAlias`与在`EncryptedSharedPreferences`例子中完全一样。最后一个参数`fileEncryptionScheme`定义了输入/输出流应该如何加密或解密的方案。目前唯一可用的值是`AES256_GCM_HKDF_4KB`。下面，我列举了一些关于它的细节:

```
EncryptedFile.Builder(
    file,
    applicationContext,
    masterKeyAlias,
    EncryptedFile.FileEncryptionScheme.AES256_GCM_HKDF_4KB
).build() 
```

主键的大小:32 字节

```
val file = File(filesDir, ENCRYPTED_FILE_NAME) 
```

HKDF 算法:HMAC-SHA256

*   AES-GCM 派生密钥的大小:32 字节
*   密文段大小:4096 字节
*   出于示例目的，我决定从示例存储库中下载`README.md`文件，使用`OkHttp`库可以访问[这里的](http://web.archive.org/web/20221205003721/https://raw.githubusercontent.com/stramek/Android-Security-Playground/master/README.md)，从响应`response.body!!.bytes()`中获取字节，然后通过将这些字节传递给以下方法来保存它:
*   然后读它:

为了加密和加载下载的文件内容，我调用了:

```
private fun onFileDownloaded(bytes: ByteArray) {
    var encryptedOutputStream: FileOutputStream? = null
    try {
        encryptedOutputStream = encryptedFile.openFileOutput().apply { 
            write(bytes) 
        }
    } catch (e: Exception) {
        Log.e(„TAG”, „Could not open encrypted file”, e)
    } finally {
        encryptedOutputStream?.close()
    }
} 
```

要在不加密的情况下加载文件以确保数据在不解密的情况下不可读，请执行以下操作:

```
private fun readFile(fileInput: () -> FileInputStream) {
    var fileInputStream: FileInputStream? = null
    try {
        fileInputStream = fileInput()
        val reader = BufferedReader(InputStreamReader(fileInputStream))
        val stringBuilder = StringBuilder()
        reader.forEachLine { line -> stringBuilder.appendln(line) }
        result.text = stringBuilder.toString()
    } catch (e: Exception) {
        Log.e(„TAG”, „Error occurred when reading file”, e)
    } finally {
        fileInputStream?.close()
    }
} 
```

以下是示例中展示其工作原理的视频:

```
readFile { encryptedFile.openFileInput() } 
```

示例项目

```
readFile { file.inputStream() } 
```

工作代码可以在下面的库[这里](http://web.archive.org/web/20221205003721/https://github.com/stramek/Android-Security-Playground)找到。

摘要

![android_security_enc_file](img/d7ac87b74d65c0d94000630892e29aa1.png)

AndroidX Security 巧妙地包装了复杂的安全逻辑，同时向开发人员公开了简单的接口。有了这个库和几行代码，就有可能使我们的应用程序更加安全，并消除开发人员忘记一些关键配置的情况。唯一的缺点是，它迫使开发者只能让应用程序兼容 android Marshmallow 和更新的版本，但记住关于`KeyGenParameterSpec`和`KeyPairGeneratorSpec`的背景故事，这个决定是合理的。此外，根据谷歌提供的[分布仪表板](http://web.archive.org/web/20221205003721/https://developer.android.com/about/dashboards)，世界上只有四分之一的设备使用 Android 5 或更旧版本，这个数字随着时间的推移正在急剧下降。我们仍然需要记住，加密项目中的`SharedPreferences`和文件是让应用程序真正安全的众多因素之一。让我们为这个库的稳定发布祈祷吧。

照片由 [克里斯·帕纳斯](http://web.archive.org/web/20221205003721/https://unsplash.com/@chrispanas) 在 [Unsplash](http://web.archive.org/web/20221205003721/https://unsplash.com/)

## Summary

AndroidX Security cleverly wraps complex security logic while exposing simple interfaces to developers. With this library and a few lines of code, it’s possible to make our application more secure and eliminate cases where the developer forgets about some crucial configuration. The only downside is that it forces the developer to make the application compatible with android Marshmallow and newer only, but keeping in mind the backstory about `KeyGenParameterSpec` and `KeyPairGeneratorSpec`, this decision is reasonable. Also, according to the [distribution dashboard](http://web.archive.org/web/20221205003721/https://developer.android.com/about/dashboards) provided by Google, only a quarter of devices in the world use Android 5 or older and this number is drastically decreasing over time. We still need to remember that encrypting `SharedPreferences` and files in a project are one of many factors that make the app truly secure. Let's keep our fingers crossed for a stable release of this library soon.

Photo by [Chris Panas](http://web.archive.org/web/20221205003721/https://unsplash.com/@chrispanas) on [Unsplash](http://web.archive.org/web/20221205003721/https://unsplash.com/)