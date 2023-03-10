# CreateML:用 Swift 开始你的机器学习之旅

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/createml-start-your-adventure-in-machine-learning-with-swift>

 CreateML 是苹果在 2018 年 WWDC 上推出的一款非凡工具，它已经引起了广泛关注。在这篇博文中，我们不仅会发现为什么会这样，还会学习如何使用这个工具。开始吧！

## 为什么这么重要？

2017 年夏天，苹果推出了 CoreML，这是一个允许你轻松地[将机器学习模型集成到你的应用](/web/20221205012646/https://www.netguru.com/services/machine-learning)中的框架。然而，这些模型可以从苹果网站下载，也可以在受支持的 Python 深度学习库(如 Tensorflow、Keras 等)的帮助下创建。关于后一个选项，您仍然需要将训练好的模型转换成。mlmodel 格式，以便将其集成到您的应用程序中。毫不奇怪，许多开发者在去年对 CoreML 的评论中提到，它并不是真正的机器学习框架，因为它不支持模型创建。今年，CreateML 改变了一切。
CreateML 框架让你无需机器学习专业知识就能轻松构建机器学习模型。在斯威夫特。在您的 Mac 上。还不够刺激吗？

## 我能做什么

创建 ML 允许您为以下内容构建机器学习模型:

*   **图像识别**

    在图片上区分猫和狗？轻松点。😅

*   **文本分类**

    现在你可以创建自己的垃圾邮件分类器🎉在斯威夫特。

*   **使用结构数据预测值**

    举例来说，你可以根据房子的面积、位置和卧室数量来预测它的价格。

## 这有多简单？

极其。让我们来看看创建 ML 的工作流程。对于那些已经有一些机器学习经验的人来说，它可能看起来很熟悉，但同时，对于那些没有经验的人来说，它并不太复杂。

工作流程由以下步骤组成:

1.  **问题**
    首先，我们需要明确需要解决的问题。关键的一步是确保机器学习可以应用于它。基本上，所有可以用 CreateML 解决的任务都在前面的段落中列出来了。
2.  **数据**
    然后，为了训练和测试我们的模型，需要收集和预处理相关数据。在大多数情况下，如果你想让你的模型成功，这是整个过程中最重要的部分。
3.  **训练**
    我们用数据训练我们的模型。然后我们要测试它。测试数据集应该不同于您用于训练的数据集，通常，该比率大约为 80/20。如果我们对测试数据集的评估结果感到满意，我们就保存模型。
4.  **改进**
    然而，如果结果看起来不太好，我们需要回到之前的步骤来改进模型。你可以在下面的图片中清楚地看到，你需要回来的步骤是准备数据。这是事实，因为这是改进模型的最流行的方法。但是，您可能需要考虑其他选项。

最流行的改进模型的方法是:

*   更改数据集。添加更多或更好的数据，仔细检查数据集中已经存在的数据。
*   使用数据增强。
*   改变训练参数——ML 算法、迭代次数等。

就是这样。如果你以前没有任何经验，这听起来可能有点难以置信，但是基本原理并没有那么复杂，相信我！这就是我们开始试用 CreateML 过程所需要知道的全部内容！

## **让我们深入研究代码吧！**

因此，在简短的理论介绍之后，我们准备深入实践部分。💪为此，我想出了一个主意:让我们创建一个应用程序来检查您的午餐是否健康！我称之为健康之轮。意想不到的名字，我知道。无论如何，这个应用程序有一个标签栏，里面有三个屏幕:图像、文本和数据表。有了这些屏幕，我们可以很容易地尝试 CreateML 中所有三种可用的功能。先说图像部分。

**免责声明:**本应用程序和 ML 模型仅用于教育目的。它们并不精确。如果你想有一个真正的成功的应用程序，这将花费你更多的时间！机器学习任务非常复杂，有时你可以花几个月的时间来训练一个模型。

**TL；博士**

图像

**任务**

### HealthyLuncher images 部分背后的想法很简单:你从照片库中上传一张照片，应用程序就会评估你的午餐是否健康。这项任务本身并不容易，但只要我们打算创建一个用于教育目的的机器学习模型，我们就可以尽可能地简化这项任务。所以，让我们假设沙拉、水果和蔬菜是健康的，而最受欢迎的快餐不是(汉堡、薯条、披萨、甜甜圈等)。).这就是我们如何创建我们的图像分类器。

**数据准备**

### 首先，你需要获取你的数据。我从*[Open Images v4 dataset](http://web.archive.org/web/20221205012646/https://storage.googleapis.com/openimages/web/index.html)*下载了沙拉、水果、蔬菜和快餐数据集。另外，我还在 *上 Google 搜索了几张图片，* 选择了选项“ *标注为重用* ”。同样，对于一个真正的商业产品，这可能是一个更困难的任务。最后，我得到了 63 张健康图片和 59 张快餐训练数据集+ 20 张健康图片和 17 张快餐测试数据集。

要记住的事情:

苹果建议每个标签至少使用 10 张图片。

*   但是图像越多越好。

*   图片越多，训练的时间就越长。

*   准备培训和测试数据集。

*   或者，您可以将一个数据集拆分为代码中的定型数据和测试数据。

*   理想情况下，训练/测试数据的比率应该在 80/20 或 70/30 左右。

*   每个标签的图片数量应该几乎相同(避免巨大的差异，比如一个标签有 50 张图片，另一个标签有 500 张图片)。

*   图像应该与真实模型所使用的图像相似。

*   提供多种图片(不同角度，光照条件等。).

*   对大小没有限制；然而，建议他们至少有 299x299 像素。

*   接下来，您只需要创建两个包含训练和测试数据的文件夹，并向其中添加子文件夹，并将类标签作为它们的名称。

**训练**

### 最有趣的部分来了。实际上，你可以在操场上用 3 行简单的代码训练你的图像分类器！

**MLImageClassifierBuilder**

### 运行操场，并向助理编辑器添加一个培训文件夹。仅此而已！你将在几秒钟内得到你的模型。

```
import CreateMLUI

let builder = MLImageClassifierBuilder()
builder.showInLineView()
```

让我补充几句关于 **的训练和验证准确性。** 这些值有助于改进算法，并通过测试精度来评估其性能。训练精度改进了算法的下一次迭代。验证准确性告诉我们是否存在过度拟合，即算法是否变得专注于特定的特征(例如，背景颜色，这实际上与手头的任务无关)。CreateML 会自动随机地将数据拆分为训练数据集和验证数据集，因此每次训练算法时，验证的准确性可能会有所不同。

花了 **15.28s** 来训练 **122 张图像。** 训练后，你可以添加一个 **测试** 数据集，如果你对结果满意，就保存模型。我们的评估显示了 97%的准确率，这是一个非常好的结果。所以这种情况下不需要改进。然而，我很好奇哪些结果没有被正确分类。理论上，你可以滚动一下，看看哪个标签错了。但是在我们的例子中，它只是显示所有的图像都被正确分类了。我猜这可能是 Xcode 的一个 bug，我已经报告过了。

之后，我们只需在操场上点击 *保存* 。新创建的模型的大小为.. **17 kB** ！👏  

**ml image classifier**

### 或者，您可以使用*MLImageClassifier*，它的工作流程与*MLImageClassifierBuider*非常相似，只是代码行更多。相对于其更简单的对应物，在*MLImageClassifier、* 中，您可以在代码中指定训练参数或将数据分为训练和测试数据。你可以说*ml image classifier*给了你更多的实验空间。如果你已经熟悉机器学习，这个选项应该会让你更感兴趣。所以，我们的模型训练看起来是这样的:

```
import CreateML
import Foundation

// Initializing the properly labeled training data from Resources folder.
let trainingDataPath = Bundle.main.path(forResource: "train", ofType: nil, inDirectory: "Data/image")!
let trainingData = MLImageClassifier.DataSource.labeledDirectories(at: URL(fileURLWithPath: trainingDataPath))

// Initializing the classifier with a training data.
let classifier = try! MLImageClassifier(trainingData: trainingData)

// Evaluating training & validation accuracies.
let trainingAccuracy = (1.0 - classifier.trainingMetrics.classificationError) * 100 // Result: 100%
let validationAccuracy = (1.0 - classifier.validationMetrics.classificationError) * 100 // Result: 100%

// Initializing the properly labeled testing data from Resources folder.
let testingDataPath = Bundle.main.path(forResource: "test", ofType: nil, inDirectory: "Data/image")!
let testingData = MLImageClassifier.DataSource.labeledDirectories(at: URL(fileURLWithPath: testingDataPath))

// Counting the testing evaluation.
let evaluationMetrics = classifier.evaluation(on: testingData)
let evaluationAccuracy = (1.0 - evaluationMetrics.classificationError) * 100  // Result: 97.22%

// Confusion matrix in order to see which labels were classified wrongly.
let confusionMatrix = evaluationMetrics.confusion
print("Confusion matrix: \(confusionMatrix)")

// Metadata for saving the model.
let metadata = MLModelMetadata(author: "Author",
                               shortDescription: "A model trained to classify healthy and fast food lunch",
                               version: "1.0")

// Saving the model. Remember to update the path. 
try classifier.write(to: URL(fileURLWithPath: "Path where you would like to save the model"),
                     metadata: metadata) 
```

可以看出，这也没那么复杂。评测性能与*MLImageClassifierBuilder*中的一样。虽然在这种情况下，不幸的是，你不能看到哪些图像被错误地分类。您所能做的就是查看 MLClassifierMetrics 的值。

**将模型添加到应用程序**

### 现在你可以添加你的了。mlmodel 文件到*healthy luncher*。完成后，如果与我的型号不同，您需要用型号名称更新*imageclassificationservice . swift*。此外，如果你的文件夹命名与这篇博文中的不同，请更改*prediction . swift*enum 中的*class labels*值。就是这样。不需要更多，让我们运行应用程序吧！

**正文**

**任务**

**数据准备**

As you can see it works pretty well! Especially if you take into account a relatively small amount of data used. The model will behave weirdly for non-related pictures and it is normal, as we didn’t train it for those pictures.

## 同样，我们可以根据哪些食物应该被指定为健康或不健康来设定规则。我认为所有煮、烤、蒸的东西都是健康的。所有油炸和油腻的东西都是快餐。在真正的应用程序中，你可能需要分析大量的信息和文本，以足够的精度对午餐进行分类。但出于教育目的，我们的数据集应该足够了。上面为图像写的大部分内容仍然与文本相关，例如在与将要使用的模型相似的数据上进行训练，数据越多越好，等等。

### 为了将数据导入分类器，您可以:

The text section in *HealthyLuncher* is  supposed to work similarly to the one on images. Basically, we expect a user to type in what they had for lunch and they will see whether it was healthy or not.

### 用一个 *json* 或者。 *csv* 格式。

放。 *txt* 文件放在带标签的子文件夹中，就像我们处理图片一样。

从代码中创建一个*ml datatable*实例。

1.  我选择了。 *json* 格式，只需手工添加数据即可。举个例子，这些是我为每个标签使用的单词:

2.  *健康:沙拉鱼蒸水煮生蔬菜水果苹果香蕉冰沙牛油果*

3.  *快餐:汉堡油炸麦当劳油炸脆皮肥汉堡披萨百吉饼面包。*

**训练**

这次的验证准确率非常低，只有 60%。那是因为用的数据。基本上，每个文本都是不同的，与其他文本无关。理想的做法是创建一个单独的验证数据集，或者简单地扩大和改进训练数据集。

**关于测试数据集，我这次还创建了一个. *json* 文件。将模型添加到应用程序**

### 再次，就像对于*image classifier*一样，需要将模型添加到 app 中！运行并测试它。

The training process looks very similar to the one we used for images.

```
import CreateML
import Foundation

// Initializing the training data from Resources folder.
let trainingDataPath = Bundle.main.path(forResource: "data", ofType: "json", inDirectory: "Data/text/train")!
let trainingData = try! MLDataTable(contentsOf:  URL(fileURLWithPath: trainingDataPath))

// Initializing the classifier with a training data.
let classifier = try! MLTextClassifier(trainingData: trainingData, textColumn: "text", labelColumn: "type")

// Evaluating training & validation accuracies.
let trainingAccuracy = (1.0 - classifier.trainingMetrics.classificationError) * 100 // Result: 100%
let validationAccuracy = (1.0 - classifier.validationMetrics.classificationError) * 100 // Result: 60%

// Initializing the properly labeled testing data from Resources folder.
let testingDataPath = Bundle.main.path(forResource: "data", ofType: "json", inDirectory: "Data/text/test")!
let testingData = try! MLDataTable(contentsOf: URL(fileURLWithPath:testingDataPath))

// Counting the testing evaluation.
let evaluationMetrics = classifier.evaluation(on: testingData)
let evaluationAccuracy = (1.0 - evaluationMetrics.classificationError) * 100 // Result: 100%

// Confusion matrix in order to see which labels were classified wrongly.
let confusionMatrix = evaluationMetrics.confusion
print("Confusion matrix: \(confusionMatrix)")

// Metadata for saving the model.
let metadata = MLModelMetadata(author: "Author",
                               shortDescription: "A model trained to classify healthy and fast food lunch",
                               version: "1.0")

// Saving the model. Remember to update the path.
try classifier.write(to: URL(fileURLWithPath: "Path where you would like to save the model"),
                     metadata: metadata) 
```

如你所见，它也工作得很好。当我输入“烤鱼和蔬菜”时，它说这是健康的，“薯条汉堡”竟然是快餐。这么🎉

### **数据表**

**任务**

**数据准备**

让我们假设所有来自火星、麦当劳、肯德基等等的东西都是快餐，在名字包含“新鲜”、“健康”、“家”等等的地方买的食物被认为是健康的。快餐很便宜，而健康食品更贵。

## 您可以像导入文本一样导入数据。这一次，我尝试了。 *json* 选项。这是文件的摘录:

### 我只用了 10 个(！)例子。看看效果好不好。

In the table screen in *HealthyLuncher* we want to tell the user whether their lunch was healthy based on three criteria: the name of the dish, the name of the producer (a cafe or a company), and the price.

### **训练**

再次说明，这个流程与前面的案例非常相似。唯一的例外是，这一次，我们将最终尝试将一个数据集分成训练集和测试集。在这里，获得了 200%的验证准确率。哇！在打印出*validation mertics . error*之后，我发现数据集不够大，无法运行验证。这很符合逻辑——它只包含了 10 个例子。显然，对于一个真实的模型，你不会想让它那样。

这次的测试评估显示了百分之百的结果。

```
[
 {
   "name": "Snickers",
   "company": "Mars",
   "price": 2,
   "type": "fast food"
 }, {
   "name": "Salad",
   "company": "Fresh",
   "price": 6,
   "type": "healthy"
 }, {
    …
 }
]
```

**将模型添加到应用程序**

### 如下图所示:又好用了。这么🎉

```
import CreateML
import Foundation

// Initializing the data from Resources folder.
let dataPath = Bundle.main.path(forResource: "data", ofType: "json", inDirectory: "Data/table")!
let dataTable = try! MLDataTable(contentsOf: URL(fileURLWithPath: dataPath))

// Spliting the data into training and testing ones.
let (trainingData, testingData) = dataTable.randomSplit(by: 0.8, seed: 5)

// Initializing the classifier with a training data.
let classifier = try! MLClassifier(trainingData: trainingData, targetColumn: "type")

// Evaluating training & validation accuracies.
let trainingAccuracy = (1.0 - classifier.trainingMetrics.classificationError) * 100 // Result: 100%
let validationAccuracy = (1.0 - classifier.validationMetrics.classificationError) * 100 // Result: 200%

// Initializing the properly labeled testing data from Resources folder.
let evaluationMetrics = classifier.evaluation(on: testingData)
let evaluationAccuracy = (1.0 - evaluationMetrics.classificationError) * 100 // Result: 100%

// Confusion matrix in order to see which labels were classified wrongly.
let confusionMatrix = evaluationMetrics.confusion
print("Confusion matrix: \(confusionMatrix)")

// Metadata for saving the model.
let metadata = MLModelMetadata(author: "Author",
                               shortDescription: "A model trained to classify healthy and fast food lunch",
                               version: "1.0")
// Saving the model. Remember to update the path.
try classifier.write(to: URL(fileURLWithPath: "Path where you would like to save the model"),
                     metadata: metadata) 
```

**更像个职业选手**

### 在这篇文章中，我们用 CreateML 用最简单的方法训练机器学习模型。然而，正如我们在文章开头提到的，CreateML 也为更多的创造力提供了一些空间。

**图像**

**正文**

## **数据表**

**优点&缺点**

### 如果我要给出 CreateML 的优点和缺点的概要，它们将如下。

Apple uses **transfer learning** when it comes to training image classification or recognition. It means that the model that we are creating has already been pre-trained with enormous sets of images. It facilitates significantly faster training and makes a model a lot lighter. But it also means that you don’t have full control over the model’s training, as you cannot run it from scratch. You could alternatively change  the training parameters, such as the number of iterations, the augmentation type used for the images, etc.

### 优点:

Just like in the image classifier, you can change the parameters. With text, you can change the  algorithm used by the classifier or specify the  language which you would like to classify. Additionally – we haven’t covered it here – there is also an  MLWordTagger out there, which enables the classification of text at the word level.

### 易于使用，不需要很强的 ML 技能。

As for data tables, we can use a classifier (for such problems as whether food is healthy or not) or regressor (for calculating values, for example, to predict house pricing). Both of them support a variety of different algorithms. **MLRegressor** supports linear regression, decision trees, boosted trees, and random forests. **MLClassifier ** offers  logistic regression, decision trees, boosted trees, random forests, and support vector machines. When you initialize *MLRegressor* or *MLClassifier*, they automatically choose the best algorithm by themselves. But if you know which one will work better for your problem or if you want to try them out, you can just initialize a specific option (ex.: *MLDecisionTreeClassifier()*).

## **高效。**

**在 macOS 上建立机器学习模型最简单的方法。**

**允许在 Swift 中构建 ML 模型。**

*   缺点:

*   当然，这不是我能判断的。然而，看起来你可以通过使用 Python 和其他专门的工具来做更多的事情。甚至我，作为一个 iOS 开发者，也注意到少了一些工具，我希望它们在那里。

*   那是因为苹果用的迁移学习。我们真的不知道那里隐藏着什么，我们也不能从头开始训练这个模型。有人可以考虑迁移学习，但并不总是如此。

*   实际上，大多数模型都是在云端训练的。当你拥有大量数据时，训练需要大量的时间和精力。如果你在云中运行一个模型，它不会影响你的计算机进程。用 *CreateML* 就没有这个选项了。然而，与此同时，它承诺有效和快速的模型训练。

**总而言之**

**何去何从？**

现在你能做的事情太多了！您可以通过使用更大的数据集或调整参数来改进模型。在 HealthyLuncher 的帮助下，您可以轻松测试您的新型号！即使是那些与健康食品无关的。您只需更改标签，就可以在应用中试用不同的型号:)

希望你和我一起享受这次 *创造 ML* 的旅程。下次见！

## **有用的材料**

*CreateML* is an awesome tool that enables [iOS developers](/web/20221205012646/https://www.netguru.com/services/ios-mobile-app-development) to start their journey with Machine Learning. It finally introduced a complete Machine Learning solution in Swift – right from creating the model to actually using it. It allows for training models by using images, text, and tabular data. The approach is very simple and easy to understand. Still, it could be improved, especially in order to become attractive for Machine Learning experts. It’s a perfect tool to start your journey with ML. However, it is not major news for pros in the field. At least, that’s how I see it for now.

## **Where to go from here?**

 There so much you can do now! You can improve the model by using larger datasets or playing around with the parameters. You can easily test your new models with the help of HealthyLuncher! Even ones that are not related to healthy food. You’ll simply need to change the labels in order to try out the different models in the app :)

I hope you enjoyed this *CreateML* journey together with me. See you next time!

**Useful materials**