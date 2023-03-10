# 如何构建可扩展的云架构？

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-to-build-scalable-cloud-architecture>

 想知道如何以及何时构建可扩展的云架构，或者为什么优化云可扩展性是有益的？从安全性和可靠性到自动化，可扩展云计算有许多优势——在这篇信息丰富的文章中，我们将深入探讨这些优势。

什么时候应该考虑可扩展的云计算？从一开始，当您设计您的[云计算解决方案](http://web.archive.org/web/20221206211327/https://www.netguru.com/blog/enterprise-cloud-computing)时。为什么？如果没有从一开始就实施可扩展的云，那么随着未来业务的增长，您将面临需要重新设计现有基础架构的风险。幸运的是，关注云的扩展并为未来的扩展做准备并不一定意味着巨大的成本和大量的时间。

在这篇文章中，我们探索了可扩展的云计算的复杂性:扩展的类型，构建可扩展的云架构，以及可以用来构建可扩展业务模型的 AWS 服务。请继续阅读真相。

## 什么是云可扩展性？

[云计算可扩展性](http://web.archive.org/web/20221206211327/https://www.netguru.com/blog/cloud-computing-scalability)是云托管环境增加或减少其“容量”以应对不断变化的负载的能力。换句话说，一个可扩展的解决方案可以适当地自我调整，以处理增加的流量，而不会过度配置，同时在负载小于正常负载时节省成本。

### 云弹性与云可扩展性

云弹性是云环境根据工作负载匹配所需资源分配的能力。弹性是云的一种属性，可帮助您在不过度调配资源的情况下满足需求，从而减少云基础架构的支出。

另一方面，云可伸缩性是指增加或减少资源分配以响应可变工作负载的能力。这是云环境的一个特性，即使负载可能增加，它也能帮助您防止系统性能下降。

### 云计算中可伸缩性的类型

尽管扩展从本质上改变了资源处理流量的能力，但有不同的方法来实现这一点。有几种类型的缩放:垂直、水平和对角线。

#### 垂直缩放

垂直缩放也称为放大或缩小。这是最基本的扩展类型，应该可以立即用于任何类型的应用程序。它是关于增加实例资源分配(即 CPU 或 RAM)而不改变你的代码或应用。

通过向承载您的应用程序的实例添加更多资源，您基本上提高了它为更多用户提供内容的能力。

水平缩放

![vertical_scaling](img/cf58f7485f4d17ad31db3982fe14f064.png)

当您考虑放大或缩小时，这就是水平缩放。这是关于增加并行工作的实例的数量，同时保持单个实例的资源分配不变。

#### 对角线缩放

对角线缩放是垂直和水平缩放的组合。当你的实例被向外扩展以处理更多的流量时，它们可以同时被向外扩展。

当一个实例上的单个进程开始消耗大部分分配给它的资源时，这种方法很有用——该实例被扩展，其余的流量由通过向外扩展产生的其他更小的单元提供服务。

![horizontal_scaling](img/1158bd868261ba5a1c824af2c6e03695.png)![diagonal_scaling](img/34c94127c8cc82a890e4fd5a1dfb1c7c.png)构建可扩展的云架构

#### 从基础设施设计过程的一开始就考虑可伸缩的架构是很重要的。为什么？可扩展的[云计算](http://web.archive.org/web/20221206211327/https://www.netguru.com/blog/advantages-and-disadvantages-of-cloud-computing)对各种规模的企业都有多重优势。

首先，可扩展的基础设施具有成本效益，因为您不必为不使用的资源付费。使用高度可扩展的云服务，您可以构建可靠的解决方案来应对工作负载峰值。

此外，由于云提供商强调基础设施的安全性，云服务中包含了安全特性。此外，推迟基础设施的可扩展性实施是一种冒险行为。为什么？

## 假设您的产品发布会取得巨大成功，并且您的应用程序用户群正在以出乎意料的速度增长。反过来，您的基础架构开始过载，您必须关闭应用程序并重新设计您的环境。像这样复杂的情况有可能会失去你的商业客户和金钱。

那么，构建可扩展的云架构的好方法是什么呢？

模块性

一切从你的应用开始，所以我们推荐模块化设计。微服务(一种模块化方法)将复杂的组件分解为不太复杂的较小部分，比整体应用程序更容易扩展、保护和管理。

集装箱化

如果你的应用是模块化的，那就容器化它。例如，使用 Docker 或 Kubernetes 之类的工具来运行您的应用程序，使您能够从单个控制面板中关注云的可伸缩性。我们还建议您让您的应用程序无状态化，并让您的应用程序映像尽可能轻量级。它使缩放变得更容易、更快且不易察觉。

### 可靠性

在大多数情况下，云服务在默认情况下是高度可用的，并提供跨区域复制。通过使用这些服务，可以最大限度地减少停机时间，并使您的应用程序更加可靠和灵活。毕竟，如果你失去了可用性，你很可能会失去信任，向一些客户挥手告别，这也意味着损失金钱。

### 监视

这是自动化的一个重要方面，使您能够理解什么可以并且应该自动化，从而最小化人为错误，同时提高一致性和速度。此外，通过监控基础架构参数并为异常状态创建警报，您能够做出数据驱动的扩展决策。

### 自动化

自动扩展让生活变得更简单，允许您在没有工程师参与的情况下扩展资源以响应流量的增加。自动扩展基于受监控的指标，是一种管理成本的便捷方式——您只需在需要时使用所需的服务器能力。还有持续集成和持续交付(CICD)要考虑，这意味着更快和自动化的应用程序部署。

### 安全性

安全性至关重要:如果您无法保护您的服务，您的业务就会受到影响。为此，必须将安全性作为初始设计的一部分来考虑。云服务有许多内置的安全特性。比如，[云计算](http://web.archive.org/web/20221206211327/https://www.netguru.com/blog/types-of-cloud-computing)像 AWS 这样认真对待数据安全的厂商，提供了身份访问管理(IAM)、安全组、CloudTrail。

### 构建可扩展计算环境的 AWS 服务

可伸缩性是云环境相对于内部部署环境的主要优势之一。有许多提供高可伸缩性和可靠性的云原生服务 ，您可以开箱即用，包括:

### **ASG(自动扩展组)**–这基本上是一组 EC2 虚拟机，是为您的应用程序提供可扩展性的最简单方式。只需为您的应用准备一个机器映像(AMI)，然后扩展策略的配置允许您使用自己的定制 CloudWatch 警报或预定义的指标(如 CPU 或网络利用率)来扩展环境。

**ECS(弹性容器服务)**–这是一种完全托管的容器编排服务，可以轻松部署、管理和扩展容器化应用。它自动负责扩展您的应用程序容器以及底层基础设施。

## EKS(弹性 Kubernetes 服务)–这种托管的 Kubernetes 服务使您可以轻松地在 [AWS](http://web.archive.org/web/20221206211327/https://www.netguru.com/featured/nodus-aws-cloud-migration) 和内部运行 Kubernetes。它还可以帮助您构建 Kubernetes 集群。

**S3(简单存储服务)**–这种高度可用的可扩展存储服务用于存储文件，并为访问频率和存储可靠性提供多种等级。它是通过按需购买模式实施的，因此数据存储永远不会过度配置。

*   **RDS(关系数据库服务)**-一种托管服务，允许您使用最流行的引擎(如 MySQL 和 PostgreSQL)来设置、操作和扩展关系数据库。纵向扩展数据库实例很容易，RDS 还为您提供了随着消耗空间的增加而动态扩展分配的存储的能力。此外，有可能使用读取器或写入器副本横向扩展数据库。
*   **cloud watch**–这种用于监控、收集指标和日志的内置服务使您能够在被监控的指标上创建自定义警报，这些警报可用于触发扩展操作。
*   根据业务需求扩展架构
*   可扩展云计算就是从一开始就尽可能以最好的方式准备您的基础设施。通过从一开始就设计和实施可扩展的云解决方案，您已经为未来的增长(或收缩)做好了准备。
*   构建可扩展的云架构并不需要花费太多的时间或太多的成本，而且它有许多优点，包括安全性和监控功能。事实上，有几个现成的 AWS 服务值得考虑，如 ASG 和 ECS。
*   无论哪种最适合您的业务需求，所有云原生服务都有一个主要优势:能够根据不断变化的负载增加或减少容量。

## 如果您想了解我们的工作以及我们如何帮助您和您的企业，请访问我们的云应用开发服务页面。

Scalable cloud computing is all about preparing your infrastructure in the best possible way, right from the outset. By designing and implementing a scalable cloud solution from the start, you're all set for the future in terms of growth (or contraction).

Building scalable cloud architecture doesn't have to be time-consuming or overly expensive, and it comes with many pros including security and monitoring features. Indeed, there are several out-of-the-box AWS services that are worth considering, such as ASG and ECS.

Whichever suits your business needs the best, there's one main advantage across all cloud-native services: the ability to increase or decrease capacity in response to changing loads.

Check out [our cloud application development services page](http://web.archive.org/web/20221206211327/https://www.netguru.com/services/cloud-application-development) if you'd like to find out what we do and how we can help you and your business.