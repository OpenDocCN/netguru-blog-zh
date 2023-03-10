# Netguru 与 MicroscopeIT 联手开展未来的深度学习项目

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/netguru-microscopeit-partnership>

 五百多张图片，三天，一个团队发布了这个应用程序的第一个版本，它将使我们的城市在照片上看起来更漂亮。Netguru 和 [MicroscopeIT](http://web.archive.org/web/20220925204058/http://www.microscopeit.com/) 联手举办了一场聚焦人工智能/人工智能的黑客大会。结果呢？一个应用程序可以通过点击一个按钮，自动从你的城市景观图片中删除所有讨厌的起重机，这一切都要归功于深度学习算法。如果你去过伦敦或华沙，你就会知道起重机无处不在！即使该应用程序的使用非常有限(老实说，你实际上不会在图片中获得这么多起重机，不是吗？)，这为两家公司未来在[机器学习项目](/web/20220925204058/https://www.netguru.com/services/machine-learning)上的合作奠定了坚实的基础。

## 探索深度学习在图像识别中的潜力

深度学习图像识别是一项肯定会塑造我们使用图像的方式的技术。它已经出现在我们日常使用的许多应用程序中，它帮助我们获得许多领域的各方面信息，无论是公共部门还是私营部门。例如， [仅面部识别技术市场预计到 2022 年](http://web.archive.org/web/20220925204058/https://www.techemergence.com/facial-recognition-applications/) 将产生约 96 亿美元的收入。它将用于医疗保健、营销、零售或安全等不同行业。很好的例子是 [自动边境控制系统](http://web.archive.org/web/20220925204058/https://www.sciencedirect.com/science/article/pii/S0167404816300736) ，它将利用不同的模式，如面部、指纹或虹膜识别来简化和加快安全程序。

![before and after pretty city 4](img/5540452ed28777837f36a88ead3b071d.png)

然而，我们仍处于探索深度学习算法的全部潜力及其在图像识别中的应用的坎坷旅程的起点。在这方面，我们需要面对许多挑战。

首先，机器学习是一个复杂的统计过程，在这个过程中，我们输入大量的样本，并试图猜测它们背后的规则。这与我们制作天气预报的方式非常相似。我们分析几十年来所有季节的温度历史数据，然后使用回归模型，我们猜测未来的天气情况。这个例子是一个简单的函数，已经在很多领域使用，但还不是深度学习。

深度学习是一个复杂的功能，其中我们利用了 [人工神经网络](http://web.archive.org/web/20220925204058/https://en.wikipedia.org/wiki/Artificial_neural_network) (ANN)。他们隐约受到了我们大脑工作方式的启发。这样的系统基于例子学习任务。简单地说，他们通过处理材料来发展他们的识别能力。

最初，人工神经网络无法在某些任务上取得与人类竞争的表现，尤其是在图像识别方面。直到 2011 年，卷积神经网络(CNN)才被引入。CNN 提供了额外的层，负责找到图片的不同部分。它们适用于处理视觉和其他二维数据。

过去几年，数据科学家一直在探索这项技术的潜力。一个在该领域拥有丰富经验的团队是来自 MicroscopeIT 的家伙，这是一家专门从事图像分析、计算机视觉和机器学习的软件公司。他们的最新挑战是在布拉格微软专注于人工智能/人工智能的黑客大会期间将深度学习应用于实践。该团队邀请 Netguru 参与这一激动人心的旅程。

![cranes ML](img/5b6df2143254597a728b8eb2f258ad0d.png)

黑客节项目

Netguru 和 MicroscopeIT 之间的合作始于我们在布拉格举行的为期三天的人工智能/人工智能黑客大会上开发的一个项目。这些团队包括来自 MicroscopeIT 的后端开发人员、Netguru 的前端开发人员和来自微软的顾问，他们在活动中一起使用深度学习和图像识别创建了一个简单的应用程序。我们做到了。

## 两支队伍在捷克首都相遇，每天都要工作几个小时。结果是一个应用程序，它首先使用深度学习来检测图像中的鹤，然后使用类似于 Photoshop 等图形编辑器中的算法来精确地将它们从照片中剪切出来。

我们的算法处理神经网络可以解决的最困难的任务之一:分割。最简单的一个——**分类**——只给你一个“是或不是”的答案(图中有/没有一只鹤)。只能了解图片是否包含某个特定的对象，而不能指定它在哪里。 **检测** 更加精确，或多或少会告诉你那个物体在哪里(例如，一个起重机在右上角)。最后我们得到了 **分割** ，这使我们能够以单像素的精度确定物体的位置。另一个高级的深度学习是实例分割，它也可以区分同一类的不同对象。

在这种类型的应用中，分段是必不可少的。我们需要知道起重机的确切位置，这样图像处理算法就可以精确地将它从照片中切掉，并用新的像素替换掉缺失的像素。最大的挑战，也是最耗时的任务，是为机器学习创建培训材料。这个过程被称为[数据注释](/web/20220925204058/https://www.netguru.com/services/data-annotation)，包括手动给数据添加描述。注释越详细，网络获得的结果越好——但这是以更耗时为代价的。

![before and after prettycity1](img/8094a6cc168afd8fcfb771dbcdb97a31.png)

该事件导致了该应用程序的首次发布。该团队将其命名为 PrettyCity: Cranes 版。该应用程序使用户能够自动删除图片中不需要的元素，并在没有不必要的噪音的情况下欣赏美丽的地标。仅在几年前，只有在一个好的照片编辑器中拥有高级技能才有可能实现的事情，现在只需点击几下鼠标，就可以花大量时间完成。

自然，并不是所有的事情都像我们期望的那样发展。上传到应用程序的一些图像对于神经网络来说更难阅读，就像下面的一个。该算法有时将起重机的一部分解释为图像的背景，所以这里我们有一个悬挂在空中的钩子。

![before and after prettycity](img/9a1654f5ac964ab415a1bcae6e363899.png)

开发者正在进行最后的润色，不久我们将向公众发布 PrettyCity 应用。

潜在用例

这款应用只是深度学习能力的一个例子。用户现在可以从城市景观中切割起重机，但算法也可以学习如何移除其他元素，以及执行不同类型的任务。

![before and after prettycity app1](img/0def9e5236af6854547bc8e20a202491.png)

改进您的假日照片

你可能已经尝试过从你的假日照片中去除多余的人群。没有人想因为一些陌生人走进相框而破坏他们度假时的美丽照片，对吗？

## 直到最近， [只有借助 Photoshop](http://web.archive.org/web/20220925204058/https://photoshoptrainingchannel.com/remove-tourists-stack-mode/) 才有可能。根据人群的密度，你需要几幅、几十幅甚至几百幅图像。然后，Photoshop 会对所有照片中的内容进行统计平均，保留相同的区域，并删除不同照片之间发生变化的所有内容。因此，它将消除移动通过所有或大多数层的对象，例如在场景中行走的人。

这种方法有两个主要的局限性:首先，你需要一个大样本的数据，其次，它不会移除所有拍摄的照片中仍然存在的物体。

### 利用深度学习算法，我们能够创建系统，该系统将识别特定类型的对象并更快地将其从您的镜头中移除，而无需拍摄多张照片，也不管该人是在移动还是静止。

计算照片上的人数

分割也可用于确定特定图片中的对象数量。当你只有 20 个甚至 50 个人的时候，在一张照片里数数人可能相对容易。但是，如果您想要验证有多少参与者参加了公共示威，该怎么办呢？

围绕这个话题，在很多场合都有一些争议。经常出现的情况是，一方组织公众游行，另一方试图通过减少参加人数来削弱游行的重要性。

使用深度学习和分割，我们的算法可以给你照片中的准确人数。

### 它对不同种类的活动也很有用。音乐节或技术会议的组织者可以决定有多少人参加了音乐会上的特定介绍。基于这样的数据，他们可以更好地了解与会者的偏好，然后计划未来的活动，首先，匹配偏好，其次，最大化场地容量。

从销售列表照片中删除敏感数据

电子商务市场是深度学习将被应用的另一个领域，二手车销售可能会特别受益于这种技术。

自动车牌识别(ANPR)已经广泛应用于高度受限区域的安全控制，如军事区或高级政府办公室周围的区域，以及停车场，以简化支付流程。这种系统首先检测车辆，然后拍摄它们。通过对获得的图像进行分割来提取车牌区域。

### 同样的算法也可以用来识别车主或经销商添加的汽车照片中的车牌，并自动将其从照片中删除。这项技术将改善许多拍卖门户网站的流程，如易贝或亚马逊，但也包括世界各地的汽车交易商。

深度学习在图像识别中的应用实际上没有边界，但仍有许多障碍减缓了这一过程。数据标注效率不高，并且显著增加了构建基于深度学习的应用所需的时间。此外，这项技术既不便宜，也不容易普及。例如，英伟达基本上是唯一一家能够生产能够处理深度学习的显卡的公司。

如果你有兴趣了解更多的技术，我们推荐你看看 [这个 quora 线程](http://web.archive.org/web/20220925204058/https://www.quora.com/How-can-you-explain-Deep-neural-networks-Machine-learning-Deep-learning-in-laymans-terms) 。

![before and after pretty city 5](img/f045743c73a5c4de5cdd59d8443b4b7a.png)

The same algorithms can also be used to recognise number plates in car photos added by an owner or a dealer and automatically cut them out of the picture. The technology will improve processes on many auction portals, such as eBay or Amazon, but also auto traders across the world.

The uses of Deep Learning in image recognition have practically no boundaries, but there are still many impediments slowing down the process. Data annotation is not efficient and significantly increases the time needed to build apps based on Deep Learning. Also, the technology is neither cheap, nor widely accessible. For instance, Nvidia is basically the only company capable of producing graphic cards that can handle Deep Learning.

If you’re interested in learning more about the technology, we recommend taking a look at [this quora thread](http://web.archive.org/web/20220925204058/https://www.quora.com/How-can-you-explain-Deep-neural-networks-Machine-learning-Deep-learning-in-laymans-terms).