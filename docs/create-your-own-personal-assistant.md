# 创建您自己的个人助理

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/create-your-own-personal-assistant>

 亚马逊 Alexa 在圣诞节过载后的崩溃就是一个证据，虚拟助手正不断变得越来越受欢迎。但是，如果你是一个电脑极客，想尽可能多地自己动手，想怎么定制就怎么定制呢？如果我告诉你，你可以建立自己的虚拟助手，并且在这个过程中充满乐趣，那会怎么样？购买 Google Home 或 Amazon Echo 就可以完成这项工作，但更令人兴奋和满意的是与你自己的设备进行交互，这些设备封装在一个别致的个性化 3D 打印外壳中！

## 所需设备

我们希望我们的助手小巧便携。

作为它的大脑，我们将使用 Raspberry Pi 3b——一种小型的、负担得起的微型计算机。如果你不熟悉树莓派，你可以在树莓派基金会的官方网站[www.raspberrypi.org](http://web.archive.org/web/20221205013617/https://www.raspberrypi.org/)上了解一下。

作为它的耳朵，我们将使用外部 USB 麦克风(除非我们有一个外部 USB 声卡)。Raspberry Pi 不提供开箱即用的语音输入支持。它有一个仅用于输出目的的 3.5 毫米插孔端口。它有几个 USB 端口，所以插入 USB 麦克风就像一个魔咒，无需任何进一步的配置。您也可以使用带有内置麦克风的 USB 网络摄像头。

最后但并非最不重要的，是一个外部扬声器，将作为它的嘴。我们可以通过 USB 或者如上所述的 3.5 mm 插孔连接到 Raspberry Pi。

## 这个计划

让我们计划一下如何使用我们的设备。

首先，我们希望能够唤醒设备开始发出命令。这叫做*语音激活*。我们希望设备能够持续监听周围环境，并等待被唤醒。让我们通过说*【嘿泰迪】*来实现它。现在，这个装置用轻柔的声音确认它已经准备好接受我们的命令。

我们可以和泰迪谈谈，希望他能理解我们。这一步叫做*语音识别*。和上一步有点不一样。一旦他知道我们说了什么，他就可以分析这个请求。如果我们问天气怎么样，他应该大声回答。为了实现这一点，我们将使用名为*的文本到语音的服务*。

我们问了一个问题，得到了答案。我们很开心，泰迪也是。他现在睡着了，但他的一只耳朵将会再次被激活并乐意帮助我们！现在，让我们学习如何完成每个步骤，让泰迪熊准备好使用。

## 语音激活

泰迪不可能一直理解我们说的每一句话，所以为了激活它，我们必须使用一个*热门词。*在我们的例子中，将会是*【嘿泰迪】*。

该设备将不断监听声音模式。一旦它检测到热门词，它将被激活，并为我们的请求做好准备。我们可以指定多个激活热门词，但不建议把数量设得太高，因为我们指定的热门词越多，它需要的处理能力就越强。

为了设置语音激活，我们将使用一个名为 [Snowboy](http://web.archive.org/web/20221205013617/https://snowboy.kitt.ai/) 的服务，这是一个热门词汇检测引擎。它提供了一个很棒的[文档](http://web.archive.org/web/20221205013617/http://docs.kitt.ai/snowboy/)，指导你如何一步一步地使用它。你甚至可以在他们的网站上记录和测试你的热门词汇。

**snow boy 库使用示例:**

```
import snowboydecoder
detector = snowboydecoder.HotwordDetector(model, sensitivity=0.5)
detector.start(detected_callback=callback_method) 
```

请记住，该设备将监听您录制的声音模式，因此，当别人说出热门词汇时，它可能无法工作。作为这个问题的解决方案，并且为了更好的模式识别，您可以使用已经被数百人训练过的通用模型之一。它包括热门词汇，如贾维斯或 T2，亚历山大。

## 语音识别和逻辑

一旦我们成功激活设备，我们就可以开始发布命令。

为了识别已发布的短语，首先需要将其记录下来。然后，我们需要将语音样本发送到外部服务，该服务将能够识别我们的短语，并以文本形式返回它。

有一个现有的 python 库，叫做 [SpeechRecognition](http://web.archive.org/web/20221205013617/https://github.com/Uberi/speech_recognition) ，可以为我们完成所有这些工作。它首先听背景噪音，所以它知道我们什么时候沉默，然后开始记录我们的声音。当我们完成时，录音会自动停止。我们现在可以使用现有的服务来识别我们的声音，如 Sphinx、谷歌语音识别、微软 Bing 语音识别或 IBM 语音到文本。一旦完成，所需的短语会以书面形式快速返回。

**演讲识别库使用示例:**

```
import speech_recognition as sr
r = sr.Recognizer()
m = sr.Microphone()
with m as source: r.adjust_for_ambient_noise(source)
with m as source: audio = r.listen(source)
word = r.recognize_google(audio, language="pl-PL") 
```

该设备现在知道确切的命令。它现在能做什么？我们想要什么都行！但是创造逻辑是我们的责任。它不会像开箱即用的谷歌助手那样智能。我们需要解析请求，并基于它执行特定的操作。由于我们使用的是树莓派，我们有可能控制各种电子设备。天空是无限的！

## 文本到语音

假设我们问泰迪外面的温度是多少(假设有一个温度传感器连接到树莓派)，我们希望大声说出结果。逻辑组成文本结果*“外面的温度是 5 摄氏度”*。为了听到答案，我们需要颠倒上一步的过程。我们必须将文本发送到外部服务，该服务将以语音格式返回短语。为了实现这一点，我们将使用[谷歌翻译文本到语音](http://web.archive.org/web/20221205013617/https://github.com/pndurette/gTTS/)，它允许指定口语文本的语言。一旦它返回结果，我们可以将声音样本保存到一个文件中，并使用扬声器播放它。

**谷歌翻译文本到语音库用法示例:**

```
from gtts import gTTS
tts = gTTS(text='sentence', lang='pl')
tts.save('/tmp/sentence.mp3')
os.system('mpg321 /tmp/sentence.mp3') 
```

## 总结

如你所见，如果我们一步一步地解决问题，事情并不复杂。它们执行不同的操作，但是结合起来，会给你无限的机会。只需在计算机上安装 Python 和所需的库，就可以开始测试所描述的服务。当一切都开始单独工作时，把它打包，作为你自己的私人助理一起工作。

* * *

照片由 [安德烈斯·乌雷娜](http://web.archive.org/web/20221205013617/https://unsplash.com/photos/39MVKfRm3TA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [下](http://web.archive.org/web/20221205013617/https://unsplash.com/search/photos/voice-control?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)