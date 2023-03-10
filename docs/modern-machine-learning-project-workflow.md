# 现代机器学习项目工作流程

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/modern-machine-learning-project-workflow>

 [所以你决定在你的应用中加入机器学习功能](/web/20221205012114/https://www.netguru.com/services/machine-learning)？太好了！第一步已经完成。毫无疑问，你可以期待你的创业有更多的可能性。

下一阶段是规划。哪种工具和算法最适合您的情况？你必须投资什么样的基础设施？实验迭代需要多长时间？在规划机器学习功能时，这些都是挑战团队的非常简单的问题。让我们假设我们有这些答案。如果我们准备好了，下一步就是开发和实验部分。

你认为最重要的问题和疑惑已经过去了吗？一点也不。精确的计划和可靠的开发过程都做得很好。今天重点讲第二部分。

我们可能会认为机器学习开发者创建了一个基线人工智能模型，训练和评估数据集，分析结果，并通过一些反馈重复这个过程。听起来没什么复杂的工作。至少在 IT 架构方面是这样。是真的。假设最基本的方法，工程师能够手动重复该过程。这听起来像是一个高效、稳定和安全的解决方案吗？我们可以做得更好。

如果你为你的机器学习项目找到一个完美的工作流程，你必须关注三个阶段:数据管理、模型/实验流程和部署。在本文中，我将介绍前两个。

## **数据**

首先，做大量的机器学习实验与我们处理大量数据的事实有关。我们经常在不同的机器和环境上，以分布式的方式与其他开发人员一起工作。想象一下数据的基本问题——与文件路径、数据版本之间的差异等有关，这并不是什么大事。尤其是在将模型初步部署到客户环境和初步验证结果之后。摆脱这些问题是个好主意。我们在 Netguru 中介绍的解决方案是**被子。**它是一个 Python 库，也是一个数据包管理的云服务。你可以在这里找到一个项目网站:【https://quiltdata.com/[。](http://web.archive.org/web/20221205012114/https://quiltdata.com/)

Quilt 是一个非常智能的 Python 包，可以与数据科学库一起流畅地工作。

软件包的作者承诺，如果我们使用该工具，数据管理将与代码管理相对接近。

它的工作方式如下:首先上传数据集，然后在 Python 中像 Python 模块一样导入数据集。这里你可以看到基本的例子:

```
$ pip install quilt
$ quilt install uciml/iris
$ python
>>> from quilt.data.uciml import iris
# you've got data
```

事实上，这建立了一种将数据作为单个输入保存的可复制方式。由于我们从单一来源导入数据，因此我们可以正确地执行版本控制任务，不会引入冗余数据。

被子与熊猫图书馆和 Jupyter 笔记本配合使用效果很好。可以看到，将数据导入到最基本的格式真的很快。

第二个优势是数据的版本控制。在 Quilt 的帮助下，您可以在数据更改、完成或更正的情况下推送同一数据集的多个版本。每个版本都由一个唯一的哈希代码表示。这给了我们在数据集版本之间跳转的可能性，类似于在 Git 中在代码版本之间签出。那看起来很有希望！我们已经看到数据管理与代码管理紧密相关。

被子的第一步将是建立您的帐户。如果你在一个项目团队中工作，可以考虑被子团队版计划。或者，您可以尝试您的个人帐户。

如果您想使用可在线访问的样本数据集，您甚至不必拥有帐户。首先，您可以通过在 Quilt 网站上搜索数据来下载数据。您应该记下数据集的用户名和名称，然后使用它通过命令在 Python 中导入数据

```
quilt install <username>/<dataset_name>
python
> from quilt.data.<username> import <dataset_name>
```

就是这样！通过一个特定的名称空间，您可以访问众所周知的 DataFrames 格式的数据，与 NumPy 和 Pandas 库兼容。

如上所述，Quilt package 提供了方便的 CLI 命令来安装和维护数据包。我们可以通过创建和部署我们自己的包来发现全部潜力。

为了实现这一点，我们应该首先构建包。我们可以从两个方面着手。

首先，我们可以通过将文本文件或图像等非结构化数据压缩到包中来隐式构建数据。假设我们有所有的数据文件(例如。csv，。txt，。jpg 文件)在与当前目录相关的子目录`/data`中，我们可以键入:

```
quilt build <our_username>/<package_name_in_quilt> ./data
```

构建本地包。然后，我们可以通过以下方式将包推送到 Quilt 存储库

```
quilt push <our_username>/<package_name_in_quilt> --public
```

就是这样！

其次，我们可以创建一个数据结构，类似于元数据文件。它是由 YAML 文件完成的。我们可以通过键入以下命令来创建示例配置文件

```
quilt generate ./data
```

它只是构建了`build.yml`和`README.md`文件。YAML 配置示例如下所示:

```
contents:
  iris:
    file: iris.data
    transform: csv
    kwargs:
      header:
      dtype:
        sepal_length: float
        sepal_width: float
        petal_length: float
        petal_width: float
        class: str 
```

然后，我们可以使用之前创建的元数据来构建和推送我们的包

```
quilt build <our_username>/<package_name_in_quilt> build.yml
quilt push <our_username>/<package_name_in_quilt> --public
```

## **实验**

我们已经解决了数据管理问题和解决方案。目前，我们有能力关心数据集的版本和集成。我们可以从不同的机器上使用它们，并与其他贡献者共享它们。

我们准备首先构建一个基线模型，然后通过迭代各种实验来改进它的性能。如果没有一个好的计划，我们将会损失大量的时间来触发机器学习框架的后续运行。完美的模型需要大量的探针。让我们更聪明地处理这项任务。

Netguru 推荐 Polyaxon，这是一个开源平台，旨在帮助机器学习生命周期管理。它支持最常见的机器学习库——Keras、Tensorflow、Caffe、PyTorch。另一方面，它允许在 Amazon Web Services、Kubernetes 和 Docker 解决方案上部署整个管理系统。

## **它的作用**

分布式管理系统是一个伟大的想法。您可以从不同的机器和配置中处理模型的教学过程。Polyaxon 统一了您的实验访问，并引入了教学任务的时间安排。它还推动了机器学习中的良好编码实践——描述模型的代码应该是无参数的——所有定义当前模型的幻数都应该作为参数传递给评估者。

让我们想象以下场景:

1.  我们创建一个基线模型，与我们的直觉相关，以实现最初的结果。我们收集准确性指标和损失值。
2.  就在观察到我们要应用微调的第一部分之后。我们重新考虑指定层中的单元数量以及其他变化，例如其他激活函数或损失函数变化。
3.  下一步是继续微调过程。我们可能需要尝试其他模型架构，其他层数，我们也可以考虑引入 LSTM、卷积层等。，取决于我们想要达到的目标。
4.  如果我们将最好的建筑本地化，我们希望保持最好的模型外观，并尝试重新考虑单元的数量和类型。在这一步中，我们只是试图取得最好的成绩，并尽量减少损失。

食谱告诉我们，我们需要评估许多不同模型的细节。这就是为什么我们想在代码中去掉硬编码模型架构细节。我们基于外部参数定义了一个抽象模型，并在内部定义了评估和训练方法。其次，我们要制定实验计划。该计划将包括一份具体实验的清单。其中每一项都将说明单位的数量和类型、激活方法、时期数和其他需要具体说明的参数。在并行计算的情况下，我们还需要为单个实验定义一些资源限制。Polyaxon 允许我们以有效的方式设置它。

## **配置**

要将我们的项目与 Polyaxon 集成，我们必须通过命令在内部创建一个新项目:

```
polyaxon project create --name="Name_of_the_project" --description="description
of the project" 
```

然后，我们可以通过以下方式启动 Polyaxon 配置文件:

```
polyaxon init "Name_of_the_project"
```

我们可以将每个实验定义为 Yaml 格式的 Polyaxon 文件。默认情况下称为`polyaxonfile.yml`。下面你可以看到一个例子:

```
---
version: 1
kind: group
framework: tensorflow

tags: [examples]

hptuning:
  concurrency: 5
  random_search:
    n_experiments: 10

  matrix:
    learning_rate:
      linspace: 0.001:0.1:5
    num_layers:
      values: [44, 45, 46]
    momentum:
      values: [0.85, 0.9, 0.93]

declarations:
  batch_size: 128
  num_steps: 5000

build:
  image: tensorflow/tensorflow:1.4.1
  build_steps:
    - pip install --no-cache-dir -U polyaxon-client==0.4.2

run:
  cmd:  python run.py    --train-batch-size= \
                         --train-steps= \
                         --learning-rate= \
                         --momentum= \ 
```

该文件指定了我们的实验要使用的框架、构建和运行命令，以及我们希望在指定实验中使用的超参数。它还允许指定要使用的资源，例如 GPU 或 CPU 内核。Polyaxon 创建了混合学习率、层数和动量值的所有乘积的实验。

如果我们准备好了所有的实验，我们可以通过以下方式将计划上传到 Polyaxon cloud:

```
polyaxon upload
```

然后我们可以通过以下方式进行实验:

```
polyaxon run -f ./polyaxonfile.yml 
```

## **总结**

在 Quilt 和 Polyaxon 工具中，您可以轻松地为您的机器学习项目设置和配置优雅的工作流。第一个服务将以类似于开发人员将库导入 Python 项目的方式提供数据。它还将保证在格式、版本和可用性方面的数据集成。第二个允许安排各种机器学习实验——训练和评估模型的下一个版本。

以下改进有助于机器学习工程师以更可重复、更快和更灵活的方式实现他的目标。