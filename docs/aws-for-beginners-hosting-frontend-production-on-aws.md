# 面向初学者的 AWS:在 AWS 上托管前端产品

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/aws-for-beginners-hosting-frontend-production-on-aws>

 亚马逊网络服务是亚马逊提供的按需云计算服务平台。简而言之，他们提供许多服务，如文件存储、数据库、DNS 和服务器，您可以在他们的基础设施上使用。

第一次在 Amazon Web Services (AWS)上配置基础设施似乎很痛苦。首先，绕过 AWS web 控制台并不容易，因为有许多服务和配置选项。尤其是当我们将其与 Netlify 或 Vercel 等替代服务进行比较时。但是真的有那么难吗？我会试着告诉你事实并非如此。

本文面向希望获得 AWS 实践经验的初学者。我将向您展示如何通过点击 AWS web 控制台为您的单页面应用程序创建基础结构。我相信 web 控制台对于第一次使用它的人来说是一个很好的介绍，因为它让你更容易理解你将要使用的所有服务。如果您想用代码定义您的基础设施，您可以在以后使用该经验。有了 Terraform 和基础设施作为代码，您将能够只通过一个命令就启动所有这些服务。

你可能会想:为什么我不能在 S3 使用静态网站服务选项？最重要的区别是成本、性能和配置选项。虽然 S3 被设计为一个简单的(因此名字中的“简单”)对象存储服务，但 CloudFront 更像是一个 CDN——它允许你在离客户端最近的地方缓存和提供你的内容。它也更便宜，并提供了许多有用的配置选项，包括强大的 Lambda@Edge。说到成本-如果你想为一个小网站服务，最贵的东西将是 53 号公路托管区-0.50 美元/月:)你可以在 AWS 网站上找到更多关于成本的信息。

## 目标

在本文中，我们将为前端应用程序创建一个全功能的生产环境。为了实现这一点，我们将把我们的域添加到 Route 53，创建一个 S3 桶、CloudFront 发行版，并生成一个 SSL 证书。我将逐步指导您完成 AWS web 控制台。按照本文中的说明进行操作后，您应该会获得一些关于 AWS 服务和 AWS web 控制台的实践知识。注意:我不会在这里讨论持续集成配置，我们将通过上传文件到 bucket 来进行部署。

开始之前，您需要:

*   有权访问名称服务器配置的域
*   AWS 帐户
*   一些要部署的应用程序(它可能只是一个 index.html 文件)

如果你想玩 AWS，你可以创建一个帐户，利用免费层([https://aws.amazon.com/free/](http://web.archive.org/web/20221205015554/https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc))，这将允许你免费测试 AWS 服务。你也可以在 freenom.com 上创建免费域名。

## 步骤 1:将域添加到路由 53

域名系统是互联网的电话簿。名称服务器的一般用途是将域名转换成 IP 地址。因此，当你在浏览器中输入 netguru.com 时，首先会有一个 DNS 服务器请求来获取该网站的 IP 地址。使用 AWS 的 Route 53 DNS 服务，你可以配置你的域名的 DNS 记录，甚至可以在那里购买一个新的域名。

我们将从向 Route 53 添加现有域名服务器开始，这将允许我们在 AWS 内管理 DNS 记录。

1.  从服务菜单中选择**路线 53**
2.  从 53 号公路左侧菜单中选择**托管区域**
3.  点击**创建托管区域**
4.  右边会出现一个表格。填写你的域名(我的是 depguru.ml)。注释是可选的，类型应设置为**公共托管区域**。填写完表格后，点击底部的**创建**。
    ![4](img/5f05776c08f9e7bb843e258443628855.png)
5.  您将被重定向到域视图的托管区域。我们稍后会回到这里。现在点击 ****回到托管区。**
    ![5](img/99da8867577c6bd75dd3fe0cc6c9666e.png)
    **

现在是时候将我们的域“连接”到我们的托管区域了。

警告: I **如果你的域名附带了其他服务——比如电子邮件或子域名——这些服务在更换域名服务器后将停止工作。所以除非你知道你在做什么，否则我建议你创建一个新的域名来玩 AWS——你可以在[freenom.com](http://web.archive.org/web/20221205015554/http://freenom.com/)获得一个免费域名。可以保留所有现有服务，但需要不同的配置。**

1.  在托管区域主页上，单击您的域(您刚刚创建的)
2.  将出现带有详细信息的侧面板。在**域名服务器**下，你可以找到你必须在你的域名配置中设置的域名服务器(ns-231.awsdns-28.com，ns-1414.awsdns-48.org……在我这里)
    ![6](img/847a7a88a0f2585ff707f6b2cc0f154e.png)
3.  在另一个选项卡中，转到您的域名注册页面(您注册域名的网站)，并前往域名设置。我从[freenom.com](http://web.archive.org/web/20221205015554/http://freenom.com/)得到了我的域名，所以我前往那里，从**我的域名**我选择了**管理域名。**您必须找到一个选项来设置自定义**域名服务器**，并为您的域名提供 53 号路线中列出的域名服务器。在某些情况下，域名服务器只有两个字段，不要担心，你可以只填写前两个。
    ![7](img/31b252e93ba33b96858b766272c19966.png)
4.  保存您的新名称服务器并返回 AWS 控制台

## 步骤 2:创建 S3 存储桶

AWS 简单存储服务，或 S3，是亚马逊的一个对象存储服务——基本上你可以在那里存储文件。S3 的物品被放进桶里。您可以将它视为文件的最高级别目录。通常我们为每个应用程序或每个上下文(环境)创建一个桶，例如 1 个用于应用程序文件，1 个用于应用程序日志。让我们创建一个存储应用程序的桶。

1.  从服务菜单中选择 S3
2.  创建一个桶
3.  在 **Name and region** 选项卡中输入一个唯一的 bucket name(我的将是: **depguru** )并选择一个 region: **EU(法兰克福)。**点击下一步
    ![1](img/5df5a47f7e2bf1617dcdb7f454008849.png)
4.  在**配置选项**选项卡中，保持所有选项不变，并点击下一步
5.  在**中设置权限**

1.  取消选中**阻止所有公共访问**
2.  勾选**我承认当前的设置可能会导致这个桶和桶内的对象成为公共的**，取消勾选后会出现阻止所有公共访问
3.  点击下一步
    ![2](img/6c34133c6e967579675c5edcb64b8ae6.png)

7.  点击**创建桶
    ![3](img/1db23056bed34f0a9e9c4cd2ac9a6029.png)** 

现在你已经把它列入了你的遗愿清单:)

## 步骤 3:请求 SSL 证书

SSL 证书支持用户浏览器和服务器之间的安全(加密)通信。它还验证网站的身份——这就是为什么它是按域名发布的。证书由证书颁发机构(CA)颁发。你可以在这里找到更多:[https://howhttps.works/](http://web.archive.org/web/20221205015554/https://howhttps.works/)

我们正在建立生产，所以 SSL 是必须的。谢天谢地，我们可以从 AWS 免费申请证书。使用 ACM——AWS 提供的一个证书管理器——您可以为您的域请求和管理 SSL 证书。

1.  在 AWS 控制台中，在**服务**菜单中找到**证书管理器**
2.  由于我们将使用 CloudFront 的证书，请确保您选择了 **N.Virginia** 地区(右上角的下拉菜单)。
3.  点击**申请证书**(注意:如果有证书也可以导入现有证书)
    ![8](img/aa5bea673c5aa6cc1a1ef1dd9580085a.png)
4.  将出现一个向导表单。选择**申请公共证书**，点击**申请证书**按钮
    ![9](img/c10116e3c00272e58d5514852a910572.png)
5.  现在您需要为您的证书指定域名。所以你需要列出你想要保护的所有域名——在我的例子中是 **depguru.ml** 和 www.depguru.ml。你可以在这里添加任何其他子域名，甚至可以添加一个通配符(*.depguru.ml)，它将覆盖所有子域名。所以指定域名，点击**下一步**。
    ![10](img/a6b6951e1f4fbbda510a5fb26366996f.png)
6.  在**选择验证方法**屏幕上，您可以选择 DNS 验证(要求您有权访问您所在域名的 DNS 区域)或电子邮件验证(要求您有权访问在 WHOIS 中作为该域名联系地址列出的电子邮件帐户)。我们将选择 **DNS 验证**，因为我们在 **Route 53** 中配置了一个 DNS 区域，这使得验证变得非常容易。
    T8![11](img/faef9ffd991ede56864a2cb090c81110.png)T10
    
7.  您可以跳过添加标签，除非您需要它们
    ![12](img/6d85f788d55be4c680ed86c1c38bc015.png)
8.  在第 4 步中，检查提供的数据，如果域名看起来正常，点击**确认并请求** **![13](img/1c277f1a94cfab3ba1344668b7ffc757.png)** 
9.  现在，您可以看到验证屏幕。
    ![14](img/d9f57331de4163095253ccb3d35320fc.png)
10.  要验证在 **Route 53** 中管理的域，只需展开每个域行并单击 **Create record in Route 53** -它将为我们添加所需的验证记录。如果您想手动添加，还会列出一个 DNS 验证记录。
    ![15](img/f7660e98a02d4ad408d3cfb139dd9f11.png)
11.  您应该会在每个域
    ![16](img/e29cbbc7f86843922aa03383699992f9.png)旁边看到成功消息
12.  之后点击**继续**。现在你会看到一个证书屏幕，在你的域名旁边有**等待验证**。这很好，你只需要耐心等待，直到你的域名被验证。您可以单击表旁边的刷新按钮，查看验证是否完成。对于我的领域使用 53 号公路只需要一分钟。
    ![17](img/00f29e8181cfa9aebd5d761e13bf78d2.png)
13.  如果您看到状态:**颁发了**旁边的您的证书，您就可以去
    ![18](img/7f039be414aa8c4da2acdb1f33362b04.png)了

## 步骤 4:创建 CloudFront 发行版

从用户的角度来看，最好是从最近的(地理上)位置接收数据。因此，如果我在波兰华沙，向北弗吉尼亚发出请求将比向法兰克福发出请求花费更长的时间。为此，我们将使用 CloudFront——AWS 的一个内容交付网络。CDN 是一组全球分布的服务器，缓存您的内容以提高最终用户的访问速度。CloudFront 使用亚马逊位于世界各地主要城市的边缘位置分发内容，以减少延迟。

我们现在已经准备好配置 CloudFront 发行版了(终于！).

1.  像往常一样，从服务菜单中选择 **CloudFront**
2.  选择**创建分配
    ![19](img/30d39d499dcea5d01f1f880122ebfec4.png)** 
3.  在下一屏的**网页**部分点击**开始
    ![20](img/9caca746b04809f51cf74ef0227d82b7.png)** 
4.  现在你会看到一个需要填写的表格。值得庆幸的是，我们将保留大多数选项的默认设置，如果您单击 **i** 符号，每个选项旁边都会有一个提示

填写**原点设置:** 

**![21](img/1b984e2c784797db1cc1475f69fc8645.png)**

1.  **源域名**:你的站点的 S3 桶(输入有建议列表)
2.  **源路径**:您想要服务的桶内的目录。我将把文件上传到 **www** 目录，所以我在这里输入 **/www**
3.  **原点 ID** 应自动填写
4.  **限制存储桶访问:**选择**是**，因为我们希望用户只能通过 CloudFront 联系到我们**(他们将无法使用直接的 S3 链接←如果你想用密码保护你的网站，这很有用)**
5.  (在**限制铲斗接近**中选择“是”后的选项)

1.  **起源:**创建新的身份
2.  **注释:**保持原样或键入任何有助于您识别此身份的内容
3.  **授予 Bucket** 的读取权限:选择 **Yes，Update Bucket Policy**——它将创建一个适当的访问策略，并将其附加到我们的 Bucket。

7.  **原点自定义头**:你可以先放着。稍后您将能够添加自定义标题

填写**默认缓存行为设置。**我将保留大多数选项的默认值，所以这里是你应该改变的:

![22](img/22cf52b2eb67a1153c21b78273f4d839.png)

1.  **浏览器协议策略:**选择**将 HTTP 重定向到 HTTPS** ，因为我们只想通过 **HTTPS** 为我们的网站提供服务
2.  **自动压缩对象:**设置为**是**将启用 gzip
3.  在 **Lambda 函数关联**中，你可以附加 Lambda@Edge 函数(想想请求和响应的钩子),比如可以要求用户输入密码。

填写**分布设置:** 

**![24](img/2591d5da233d4db911bcd637cd8008c1.png)** ****价格等级**:选择您想要部署的边缘位置。更多地点→更高成本，因此我将选择**仅使用美国、加拿大和欧洲**，因为我不会将我的应用定位于亚洲或非洲。

**![distributionv2](img/8c8a5b55805e9994671aefdc61ac8f87.png)**

**AWS WAF Web ACL** :跳过

1.  **备用域名(CNAMEs):** 在此输入您的域名(用逗号或逐行分隔)。我的情况: **depguru.ml** **，** **www.depguru.ml** **。如果你没有自己的域名，你可以使用像 d12323.cloudfront.net 这样由 Cloudfront 生成的域名，但这并不适合生产**
2.  **SSL 证书:**选择**自定义 SSL 证书**，然后在下面的输入中选择我们之前生成的证书
3.  定制 SSL 客户端支持:保持原样(SNI)，因为 LCI 每月花费 600 美元 xD
4.  **安全策略:**原样
5.  **支持的 HTTP 版本:**原样(HTTP/2 …)
6.  **默认根对象:**用户请求“/”时返回的文件。在我们的案例中，index.html 的
***   **日志记录:**选择是否希望 CloudFront 记录请求——然后它会创建包含所有应用请求的日志文件，并将它们保存在所选的存储桶中。把它投入生产可能是个好主意，但你将为此付费。*   如果您在选择记录:**，您还必须选择:***   **日志桶** : S3 桶，你的日志将存放在这里。为此创建一个单独的存储桶可能是有意义的。我有**的书面陈述**。*   **日志前缀:**您可以在存储桶中指定存储日志的目录

1.  **Cookie 记录**:如果你选择**是**，客户端的 Cookie 将被包含在日志文件中——我们可能不需要，所以我选择**否**
2.  **启用 IPv6:** 可以，为什么不可以
3.  **评论:**你可以添加任何你喜欢的评论。

*   **发布状态:**如果你想马上发布 CloudFront 发布——选择**启用**。否则，您将能够在以后启用它*   现在，您可以单击**创建分布**来提交表单。之后，在 CloudFront 部分的左侧菜单中选择**发行版**，你就可以看到你的发行版了。
    *   **Distribution State:** If you want to publish CloudFront Distribution right away - select **Enabled**. Otherwise you will be able to enable it later**

 **步骤 5:点域到 CloudFront 分发

![25](img/9b837a9da43d381d3e93bc0a1f636df1.png)

我们必须使我们的域指向 CloudFront 分布。为此，我们将在 Route 53 中为我们的域创建 DNS 记录。如果你想了解更多关于 DNS 记录的信息，看看[https://www.cloudflare.com/learning/dns/dns-records/](http://web.archive.org/web/20221205015554/https://www.cloudflare.com/learning/dns/dns-records/)。我们将创建别名和 AAAA 别名记录——一般来说，A/AAAA 记录指向 IP 地址，但 AWS 也提供别名记录，以便使用域名将流量路由到 AWS 资源。你可以在这里阅读更多:【https://aws.amazon.com/route53/faqs/[。](http://web.archive.org/web/20221205015554/https://aws.amazon.com/route53/faqs/)

## 让我们使用服务菜单返回到 Route53。在 **Route53 Hosted zones** 区域点击你的域名(在我的例子中是:depguru.ml)。

We have to make our domain point to CloudFront distribution. To do that, we will create DNS records for our domain in Route 53\. If you want to learn more about DNS records, take a look at [https://www.cloudflare.com/learning/dns/dns-records/](http://web.archive.org/web/20221205015554/https://www.cloudflare.com/learning/dns/dns-records/). We will create A alias and AAAA alias records - in general A/AAAA records point to IP addresses, but AWS also offers alias records to route traffic to AWS resources using a domain name. You can read more here: [https://aws.amazon.com/route53/faqs/](http://web.archive.org/web/20221205015554/https://aws.amazon.com/route53/faqs/).

Let’s go back to Route53 using the services menu. In the **Route53 Hosted zones** area click on your domain name (in my case: depguru.ml).

我们需要为每个子域创建 2 个记录集(IPv4 和 IPv6)。我有 2 个子域名(depguru.ml 和 www.depguru.ml ),所以我将总共创建 4 个记录。

![26](img/0421d1d1e99d16ebaa38b1e623e3d138.png)

因此，重复下列步骤:

主域(在我的例子中是 depguru.ml)

名称:<leave empty=""></leave>

*   类型:A 和 AAAA

*   子域(www.depguru.ml)
*   名称:www

*   类型:A 和 AAAA

*   点击**创建记录集**。带有窗体的侧面板会出现
    ![27](img/0b3c499b9ca04fd75353713e7873b403.png)
*   填写一个名称(如果您想指向主域，可以留空——在我的例子中是 depguru.ml)

1.  选择一种类型(上面列出)
2.  别名:**是**
3.  别名目标:从自动建议列表
    ![28](img/06edb963980af85b8a7ef843ff16702c.png)中选择您的 CloudFront 发行版
4.  单击创建
5.  之后你应该有 4 A/AAAA 唱片。

6.  click Create

但是等等，这是怎么回事？

![29](img/bfc5e3f5ef5a6c48c03f207e91158796.png)

我们已经将 DNS 服务器设置为将用户重定向到我们的 CloudFront 发行版。

所以当用户询问时

*>在哪里可以得到**depguru . ml**网站？*

他会得到答案:

*>一个< IP 的 CF 分布>*[](http://web.archive.org/web/20221205015554/http://d108wexfud5bxn.cloudfront.net/)

我们必须为 IPv4 和 IPv6 以及 www 设置它。子域，因为我想让 www.depguru.ml 指向我的 CloudFront 发行版。(注:www.depguru.ml 和 depguru.ml 实际上是不同的域，可以指向两个不同的站点)。

步骤 6:在 CloudFront 中处理 SPA 路由

我们的应用程序现在工作，但由于这是一个单页应用程序的前端照顾路线。因此，当用户使用“/”路径进入应用程序时，它可以正常工作，但当它从另一个页面(如“/users”)开始时，就不能工作了。这是因为 CloudFront 在/(我们将默认根对象设置为 index.html)上提供 index.html，但不知道如何处理其他路径。

## 我们可以通过设置后备来解决这个问题——告诉 CloudFront 如果找不到文件就重定向到 index.html。这可能不是最好的解决方案(在某些情况下可能会损害 SEO，因为它可能会为不存在的页面返回 200 ),但这是最简单的，所以我会用它。

要将错误页面重定向到索引，您必须:

在 AWS 控制台中，从服务菜单中选择 **CloudFront**

从 CloudFront 分发列表中选择您的并点击**分发设置**

**![39](img/5e05ba81886f0e1b36d43bb52cc7c581.png)**

1.  转到错误页标签
    ![40](img/7c324a86d67bab90e51e98079f0eca39.png)
2.  点击**创建自定义错误响应**，填写表格
    ![41](img/c047580a60145b318ad824b706117d8a.png)
3.  **Http 错误代码:** 403 禁止
4.  **自定义错误响应:**是
5.  **应答:** /index.html
6.  **HTTP 响应代码:** 200 OK
7.  点击**创建**和对**重复这些步骤**Http 错误代码: 404
8.  **HTTP Response Code:** 200 OK
9.  Click **Create** and repeat those steps for **Http Error Code:** 404

之后，如果你直接去 https://depguru.ml/about，你应该还能看到我的应用。

![42](img/33758702aed20199af20fb96d1e2d851.png)

步骤 7:部署和测试

现在，我们的基础架构已准备好提供内容…但我们还没有内容！如果你现在访问你的域名，你可能会看到来自 S3 的拒绝访问错误。

## 我们现在需要将我们的应用程序部署到 S3 木桶中。为了简单起见，我将在 AWS 控制台中转到 S3，并使用 GUI 将文件上传到我的 bucket 中的 www 目录。

从服务菜单中选择 S3

选择您以前创建的桶(在我的例子中是 depguru)

1.  点击**上传**
    ![30](img/a3632186c71aa5ecfaa1e59f6b6a06dc.png)
2.  将您的 **www** 目录与应用程序内容拖放在一起(如果是 React:运行 yarn build，将构建目录重命名为 www 并拖动到浏览器的 bucket 中)。
    T3![31](img/30ab865b7d9f82cf603239f3bfc82ece.png)
3.  点击**下一个**
    ![32](img/1c4b18910ad2a7118f670cbc89681489.png)
4.  在**设置权限**画面上留下默认选项，点击**下一步**
    ![33](img/c0160428aac955290259b94a296ee125.png)
5.  在**设置属性**屏幕上保留默认选项(标准)并点击**下一步**
    ![34](img/2f4f6db07db11baf921f106fca09627a.png)
6.  点击**上传**
    ![35](img/a4ff6a2953298eb4a1d59657f8aedc33.png)
7.  现在您应该看到 www 目录及其内容。

8.  Click **Upload**
    ![35](img/a4ff6a2953298eb4a1d59657f8aedc33.png)

Now you should see the www directory and its contents.

![36](img/00a516f6a3f0ebcf3a5a10a5e6f61245.png)

之后，我的应用程序可以在 depguru.ml 上找到

![37](img/1d151523ddea7bc8be614b394f4e8b2d.png)

注意:DNS 更改的传播可能需要时间(最多 24 小时)，所以如果您不能立即看到您的站点，请不要惊慌。

摘要

所以现在我们在 AWS 上有了一个全功能的生产环境！这并不难，是吗？我第一次尝试花了大约 30 分钟来设置一切。AWS 控制台看起来比实际上更可怕。

## 当然，很难在一篇文章中包含所有的 DevOps 和 AWS 知识，所以我不得不在这里或那里简化一些东西，并选择一个简单的用例来展示基本原理。但我仍然认为，如果你对这个主题感兴趣，这是一个很好的起点。总的来说，用代码创建基础设施更好(参见:Terraform ),但是如果你是用 AWS 迈出第一步，我认为使用 GUI(本文中所示的方式)是一个很好的起点。

注意:小心凭证和选择选项。AWS 向你收取使用费，所以做一些愚蠢的事情可能会导致月底的巨额账单。此外，如果你的凭据在某个地方泄露，有人可以使用你的帐户(和信用卡)进行类似挖掘加密货币的活动。

Of course it is hard to contain all DevOps and AWS knowledge in one article, so I had to simplify things here and there and chose a simple use case to show the basic principles. But I still think it's a good starting point if you are interested in the subject. In general it is better to create infrastructure with code (see: Terraform) but if you are taking your first steps with AWS, I think using the GUI (the way shown in this article) is a good starting point.

Note: Be careful with credentials and selecting options. AWS bills you for usage, so doing something stupid may result in a huge bill at the end of the month. Also if your credentials leak somewhere, someone can use your account (and credit card) for things like mining cryptocurrencies.****