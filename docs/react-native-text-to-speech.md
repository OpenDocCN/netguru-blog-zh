# React Native 的文本到语音解决方案的比较

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/react-native-text-to-speech>

 文本到语音解决方案的比较

市场上有许多文本到语音的解决方案，从谷歌云文本到语音，微软 Azure 文本到语音，亚马逊 AWS Polly，到 Android 和 iOS 设备的原生实现解决方案。虽然最后一种似乎更容易实现，更可靠，并提供了在没有互联网连接的情况下使用它的可能性，但“更大的”[解决方案由机器学习](/web/20221205013216/https://www.netguru.com/services/machine-learning)支持，并提供更高质量的语音。最大的缺点是对 React Native 的支持——虽然他们都有官方的 iOS 和 Android SDKs，但真的很难找到 React Native 的库。在本文中，我们对普通 React 本地应用、Expo 本地应用和 Google 语音转文本的 Rest API 的本地解决方案进行了比较。

## React-Native-TTS 库

在 [React Native apps](/web/20221205013216/https://www.netguru.com/blog/react-native-apps) 中实现文本到语音功能的最简单方法是使用 React-Native-TTS 库。它非常容易设置，并且该库可以与 React-Native 版本 0.60+的自动链接一起使用。
这种解决方案的最大优势是什么？两个平台上的原生文本到语音引擎允许您在应用程序中轻松使用该功能，更重要的是，允许您在没有网络连接的情况下使用一些语音。

这种解决方案的缺点是缺乏一致性。Android 和 iOS 有不同的文本到语音引擎，所以每个平台对语音的选择是不同的。如果你想创建一个会说话的聊天机器人，这可能是一个问题。

你可以在 Android 上选择 281 种不同语言的语音，在 iOS 上选择苹果提供的 38 种。

在配置您的声音时，您可以设置如下参数:

*   语言

    ```
    Tts.setDefaultLanguage('en-IE');
    ```

*   声音

    ```
    Tts.setDefaultVoice('com.apple.ttsbundle.Moira-compact');
    ```

*   语速

    ```
    Tts.setDefaultRate(0.6);
    ```

*   音高

    ```
    Tts.setDefaultPitch(1.5);
    ```

在 Android 上还有平移和音量:

```
Tts.speak('Hello, world!', { androidParams: { KEY_PARAM_PAN: -1, KEY_PARAM_VOLUME: 0.5 } });
```

设置非常简单，只需输入:

```
yarn add react-native-tts
```

并且库应该在运行 pod install 时链接它自己。

让我们创建一个简单的语音转文本组件:

```
 import React from 'react'
import { Button } from 'react-native'
import Tts from 'react-native-tts'

Tts.setDefaultLanguage('en-GB')
Tts.setDefaultVoice('com.apple.ttsbundle.Daniel-compact')

const NativeSpeech = () => (
 <Button
   title="Speak!"
   onPress={() => Tts.speak('Hello World!')}
 />
)

export default NativeSpeech 
```

## 语音反应本地 Expo 模块

如果你使用 expo，React-Native-TTS 库不适合你。相反，你应该使用 [Expo-speech SDK](http://web.archive.org/web/20221205013216/https://docs.expo.io/versions/latest/sdk/speech/) 。你只需要跑:

```
expo install expo-speech
```

如果您想在非 expo 项目中使用这个库，您可以使用 unimodules 包。只需遵循 unimodules 文档，运行 expo install expo-speech，然后运行 pod install。

expo 的语音库使用与 React-Native-TTS 库相同的本地文本到语音引擎，因此您可以使用与该库相同的语音，配置过程也是类似的。更多信息，请访问世博会演讲文档。
让我们用世博演讲服务创建一个简单的组件:

```
 import React from 'react'
import { Button } from 'react-native'
import * as Speech from 'expo-speech'

const NativeSpeech = () => (
 <Button title="Speak!" onPress={() => Speech.speak('Hello World!')} />
);

export default NativeSpeech 
```

## 谷歌云语音转文本

Google Cloud 语音转文本是一个没有 React Native 客户端库的解决方案。我们可以使用它的 rest API 请求 base64 编码的 mp3 和我们的文本。这项服务的最大优势是能够选择一种在两个平台上都可用的声音。它也是高度可配置的。您可以从标准声部集合中选取，但也可以从更高质量的声部库中选取。

前往[谷歌文本语音网站](http://web.archive.org/web/20221205013216/https://cloud.google.com/text-to-speech/)。您可以在下面的面板中测试所有可用的声音。

![Screenshot 2019-12-17 at 14.32.07](img/1a9e3080f0edaaa77e2eb905122fa161.png)

要开始使用文本到语音转换，请点击“免费开始”按钮并创建一个帐户。

如果您准备好了，那么在开始之前要做的第一件事就是创建一个 API 密匙。当然，你可以创建一个简单的 API 键来测试文本到语音的转换，但是如果你想在一个真正的应用程序中使用它，你应该像这篇博文中描述的那样限制这个键。

打开[谷歌云控制台](http://web.archive.org/web/20221205013216/https://console.cloud.google.com/)，从菜单中选择“API&服务”，然后前往凭证部分。单击“创建凭据”并使用“API 密钥”。将创建密钥；点按“限制键”按钮。您将看到这样一个屏幕:

![Screenshot 2019-12-17 at 14.39.28](img/52d067b807402426c38c762e44fce3cf.png)

为您的密钥选择一个名称。我们将为 Android 和 iOS 应用程序分别创建一个密钥，因此您可以将它们命名为“API key - iOS”和“API key - Android”。然后转到应用程序限制:

![Screenshot 2019-12-17 at 14.40.03](img/5beec270cbcc7566ec28805050e86e3a.png)

您可以为您的应用捆绑包限制您的 API 密钥。它将防止攻击者窃取您的密钥，并在您的应用程序之外使用它们。你得准备两把不同的钥匙，一把安卓的，一把 iOS 的。如果您愿意，您也可以将这些按键限制为“仅文本到语音”:

![Screenshot 2019-12-17 at 14.42.22](img/4e1a403f3fc635039487e51b70c3050a.png)

现在，您可以使用 React-Native-Config 库将您的密钥安全地存储在您的 ENV 文件中，然后根据平台提取 API 密钥。 

Google 的语音转文本是一个 REST API，没有 React Native 的客户端库。这就是为什么我们需要获取 base64 编码的语音到我们的应用程序，然后播放它。

使用这种解决方案的一个巨大优势是由机器学习支持的更高质量的语音。您可以从每种语言的大量声音列表中进行选择。每种语言都有几种声音，男性和女性都有，但也有标准声音和 WaveNet 声音，因为语音质量更好，所以价格稍微高一点。你可以在谷歌文本到语音转换的参考列表上查看它们。如何设置自己选择的声音？就像把设置添加到一个 API 调用体中一样简单:  

```
 voice: {
     languageCode: 'en-US',
     name: 'en-US-Standard-B',
     ssmlGender: 'FEMALE'
   }
```

让我们创建一个 API 调用来获取这些数据。我们的目标是从 https://texttospeech.googleapis.com/v1/text:synthesize 端点获取数据，所以首先，我们可以创建一个函数，用我们的头和体返回一个对象:

```
 const createRequest = text => ({
 headers : {
   'Content-Type': 'application/json'
 },
 body: {
   input: {
     text
   },
   voice: {
     languageCode: 'en-US',
     name: 'en-US-Standard-B',
     ssmlGender: 'FEMALE'
   },
   audioConfig: {
     audioEncoding: 'MP3',
   }
 },
 method: 'POST'
})
```

createRequest 函数有一些针对语音的设置和针对音频类型的配置，但最重要的是“文本”参数，这是我们希望 Google 说的文本。现在我们可以创建一个函数来获取我们的 mp3。

```
 import Config from 'react-native-config'
import { Platform } from 'react-native'

const speech = async (text) => {
 const key = Platform.OS === 'ios' ? Config.KEY_IOS : Config.KEY_ANDROID
 const address = `https://texttospeech.googleapis.com/v1/text:synthesize?key=${key}`
 const payload = createRequest(text)
 try {
   const response = await fetch(`${address}`, payload)
   const result = await response.json()
   console.log(result)
 } catch (err) {
   console.warn(err)
 }
} 
```

如您所见，我们使用 react-native-config 根据当前平台获取 API 键，使用 createRequest 函数传递文本，然后从 Google Text to Speech API 获取。它应该向控制台返回 base64 编码的数据，只需运行:

```
speech(‘Hello world!’)
```

如果我们想要解码 base64 并在我们的设备上播放它，我们需要使用 react-native-fs 库，因此我们可以创建 createFile 函数:

```
 const RNFS = require('react-native-fs')

const createFile = async (path, data) => {
 try {
   return await RNFS.writeFile(path, data, 'base64')
 } catch (err) {
   console.warn(err)
 }

 return null
} 
```

作为第一个参数，我们需要传递路径和 base64 编码的数据。它会将它作为 mp3 保存在我们的设备内存中，可以通过 React-Native-Sound 库播放。所以现在我们需要一个函数来播放我们的声音:

```
 const Sound = require('react-native-sound')

const playMusic = (music) => {
 const speech = new Sound(music, '', (error) => {
   if (error) {
     console.warn('failed to load the sound', error)

     return null
   }
   speech.play((success) => {
     if (!success) {
       console.warn('playback failed due to audio decoding errors')
     }
   })

   return null
 })
}
```

我们需要将之前创建的路径作为参数传递给 playMusic 函数。现在，我们可以连接我们的代码并尝试我们新的语音服务:

```
 const speech = async (text) => {
 const key = Platform.OS === 'ios' ? Config.KEY_IOS : Config.KEY_ANDROID
 const address = `https://texttospeech.googleapis.com/v1/text:synthesize?key=${key}`
 const payload = createRequest(text)
 const path = `${RNFS.DocumentDirectoryPath}/voice.mp3`
 try {
   const response = await fetch(`${address}`, payload)
   const result = await response.json()
   console.log(result)
   await createFile(path, result.audioContent)
   playMusic(path)
 } catch (err) {
   console.warn(err)
 }
}
```

## 摘要

在本文中，我们比较了语音到文本的解决方案——本地解决方案和谷歌云语音到文本解决方案。我们还提供了一些关于在基于 Expo 的项目和本地项目中使用该服务的信息。

在选择语音转文本解决方案时，我们必须决定最重要的功能——是离线使用这项服务，还是在所有平台上使用相同的声音？也可以同时使用这两种解决方案，将原生解决方案作为后备方案，因此可以随意试验代码！

照片由丹·法雷尔在 Unsplash 上拍摄