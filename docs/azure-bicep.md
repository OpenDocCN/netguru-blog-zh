# 使用 Azure Bicep 增强您的基础架构即代码

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/azure-bicep>

 Azure Bicep 语言已经成为 ARM-JSON 格式的流行继任者，用于在 Azure 中声明资源。它克服了阻碍开发人员的一些关键问题，并帮助 DevOps 工程师和开发人员创建更加灵活、适应性更强、创作和部署速度更快的基础设施即代码。

虽然微软 Azure 资源管理器模板很好地满足了他们的目的，但 JSON 格式也带来了一系列的困难。Bicep 的开发是对这些困难的直接回应，它使用了一种不同的语言。

Azure Bicep extension for infra structure-as-Code(IaC)项目的流行源于其简化的语法以及与 Azure CLI 和 Visual Studio 代码的卓越集成能力。更重要的是，现有的 ARM 模板仍然可以使用，因为在 Azure Bicep 中创建的任何东西在部署时都将被传输到现有的 ARM 模板中。

因此，考虑到这些一般的好处，让我们更详细地看看 Azure Bicep 的特性，并了解它如何帮助我们加强我们的基础设施即代码。

## 什么是 Azure 二头肌？

Bicep 是一种特定于领域的语言(DSL ),用于以声明方式将 [Azure 资源](/web/20221206204748/https://www.netguru.com/blog/azure-architecture)(在本例中，这些资源是[基础设施](http://web.archive.org/web/20221206204748/https://www.netguru.com/blog/cloud-infrastructures-preventing-outages) e)部署到 Azure。与 Microsoft Azure 资源管理器(ARM)模板相比，Azure Bicep 语法简化且简洁，这使得它更容易学习和使用。因此，这是介绍基础设施即代码的可能性的一个很好的基础。

由于 Azure Bicep 基础设施可以在项目的整个开发生命周期中持续使用，因此您可以确保您的基础设施保持一致。它支持代码重用，无需复制和粘贴，具有改进的类型安全性，并更好地支持模块化，从而带来卓越的创作体验。Bicep 是开源的，因此可以在 GitHub 上免费访问源代码——由于访问如此方便，它的开发社区一直在增长。

### 二头肌与手臂模板

Azure Bicep 是创作传统 Microsoft ARM 模板的可行、有效的替代方案，原因如下:

*   Bicep 是对 ARM 模板的透明抽象，这意味着在我们所知的临时限制之外，可以在 ARM 模板中完成的任何事情都可以在 Bicep 中完成。
*   ARM 模板有效的资源类型和内置函数也可以从一开始就与 Bicep 一起使用。
*   Bicep 代码被编译(或传输)成标准的 ARM 模板 JSON 文件，这有效地将 ARM 模板视为一种中间语言。
*   与 ARM 模板的 JSON 格式相比，Bicep 提供了更简洁、干净的语法。在某些情况下，二头肌大约是同等手臂模板的一半大小。这是因为 Bicep 中的表达式、参数、字符串、逻辑操作符、部署范围、变量等等需要更短的代码。

### 蓝色二头肌的好处

我们来看看 Azure Bicep 更一般的好处。

*   **支持所有资源类型和 API 版本。**针对 Azure 服务的预览版和 GA 版一经提供就得到 Bicep 的支持，无需任何更新。
*   **简化的语法。**如上所述，与同类产品相比，Bicep 使用了更简单、更清晰的语法风格。
*   **模块化。** Bicep 代码可以分为易于管理的模块，每个模块部署一组相关资源。这简化了开发过程，并在重用代码时加快了过程。
*   使用 Visual Studio 代码(VS 代码)的一流创作体验。当 VS 代码被用作您的编辑器时，它提供了丰富的类型安全、智能感知和语法验证。对 Visual Studio 2022 的支持正在进行中。
*   **幂等文件。** Bicep 文件可以反复部署以实现相同的资源类型和状态——不需要开发许多文件来表示开发生命周期后期的更新。
*   没有要管理的状态或状态文件。由于所有的状态数据都存储在 Azure 中，多个用户可以在开发中协作，并确信每个单独的更新都会按预期得到处理。
*   **资源经理。** Azure Bicep 的资源管理器简化了订购操作的过程，只需一个命令即可实现完全部署。相互依赖的资源以正确的顺序部署；并且，如果可能的话，为了更快地完成，并行地进行。
*   在微软的支持下开源。 Bicep 可以免费安装和使用，高级功能无需付费。由于它也是微软拥有的产品，用户有一个可靠和专业的支持团队的好处。

## 如何用 Azure Bicep 部署资源？

Azure 二头肌的好处显而易见。因此，下一步是开始部署资源——但是如何做呢？部署 Azure Bicep 代码涉及哪些步骤？你怎么知道你的代码是正确的呢？

### 资源与模块

部署范围。理解使用资源和使用模块之间的区别很重要，并确定您的部署重点——这是您的部署范围。资源的部署范围是资源组、订阅、管理组或租户。

资源。资源是由 Azure 管理的实体。虚拟机、虚拟网络、存储帐户甚至资源组或订阅都是 Azure 资源的示例。在 bicep 中，资源由一个资源声明来表示，该声明以一个 resource 关键字开始，后跟一个符号名称(用于在 Bicep 中的其他资源或模块中引用——不要与资源名称混淆，后者是在资源主体中定义的)、资源类型和 API 版本，以及一组根据您的需要来调整它的属性。

子资源。这种类型的资源仅存在于另一个资源的上下文中——一个资源实际上是另一个资源的子资源。对于不同类型的父资源，哪种类型的资源可以成为子资源是有限制的。

扩展资源。这种类型的资源修改另一种资源。例如，为资源分配访问角色或诊断设置。

模块。可以使用 Bicep 将部署组织成模块。每个模块都是从另一个 Bicep 文件部署的 Bicep 文件。模块使你的二头肌文件清晰易懂，但是把复杂的细节和内容组合在一起。模块可以重用，并与组织中的其他开发人员共享。从技术上讲，模块(或者更一般地说，任何部署)是 Microsoft.Resource/Deployments 类型的资源。Bicep 模块被转换成一个带有嵌套模板的 Azure 资源管理器模板。嵌套模板实际上是在另一个 ARM 模板中编写的 Microsoft.Resource/Deployments 类型的资源，它的一个参数采用应该部署的 ARM 模板。一个模块可以是一个本地文件、模板规范或者到 Bicep 注册表的链接。模块还允许您部署到不同于调用模块的范围。

### 参数与变量

建立二头肌文件的参数是下一个重要的步骤。不同的参数值将使您能够在不同的环境中重用 Bicep 文件。您可以使用关键字“param ”,后跟参数名称、类型和可选的默认值来定义它们。此外，您可以注释参数以设置以下一项或多项:

*   字符串或对象是否安全
*   部署中允许哪些值
*   字符串长度的约束
*   最小和最大整数值

开发人员还应该包括对参数的描述，这样就可以很容易地看出它的用途。您还可以添加元数据参数来定义任何值或类型的自定义属性。若要减少模板中使用的单个参数的数量，请将相关值作为对象传入。

另一方面，变量用于简化 Bicep 文件的开发。变量可以用来包含复杂的表达式，然后可以在需要时作为一个整体放入。资源管理器在部署开始前解析这些变量，用它们代表的复杂表达式替换它们。与 params 相反，在设置变量时不需要指定数据类型，因为它由它包含的表达式和值所隐含。

### 引用资源

您可以在模板文件中引用资源，以便管理资源的部署顺序，例如，在部署数据库之前部署逻辑 SQL server 可能是很重要的。为此，您必须使一个资源依赖于它需要跟随的资源。

您可以使用点语法来访问资源属性和模块输出。VS 代码扩展对于查看资源的所有可用属性也非常有帮助(尽管重要的是要记住源数据并不总是 100%准确的)。

资源依赖有两种形式:隐式依赖和显式依赖。隐式依赖意味着一个资源声明引用同一个部署中的另一个资源——在这种情况下，显式依赖就没有必要了。

显式依赖关系使用 dependsOn 属性来描述资源之间的关系。然而，应该尽可能避免使用它们——相反，通过它们的符号名来暗示依赖关系。您可以在 Visual Studio 代码中可视化您的依赖关系网络。

通过使用' existing '关键字和资源的符号名，可以引用当前 Bicep 文件中没有部署的资源。同样，您还必须确定资源是在相同的范围内还是在不同的范围内。

### 条件句和循环

您需要能够在 Bicep 中单独部署资源或模块——条件允许您这样做。使用“if”关键字确定资源或模块是否已部署；然后，在部署期间，条件值将解析为 true 或 false。

请注意，条件只能添加到整个资源或模块中。可以使用参数建立条件，也可以考虑依赖关系、引用和列表函数。若要有条件地设置属性值，可以使用三元运算符:“condition？值-如果-真:值-如果-假。

迭代循环可以定义资源、变量、模块、属性或输出的多个副本，以帮助您避免语法重复，并设置部署期间需要创建的副本数量。

他们使用“for”关键字。循环可以由整数索引(用于创建特定数量的实例)、数组中的项(用于每个元素的实例数量)、字典对象中的项(用于对象中每个项的实例)、整数索引和数组中的项以及条件部署来声明。

### 编译和部署

为了进行部署，您的 Bicep 文件需要转换为 ARM 模板。这是使用‘build’命令完成的——但是，一般来说，这是自动完成的，你不需要手动运行命令，除非你想预览 ARM 模板 JSON。命令“decompile”将 ARM 模板 JSON 转换回 Bicep。

要将您的资源部署到 Azure，您的 Bicep 文件必须存储在本地，并且您必须使用最新的 Azure CLI。必须设置正确的必需权限，包括对将要部署的资源的写访问权限以及对 Microsoft.Resources/deployments 资源类型的所有操作的访问权限。

如上所述，建立您的部署范围和参数。您可以预览部署的效果，并通过运行“假设”操作来验证 Bicep 文件中的错误。请确保为您的部署指定一个反映其用途的名称。

### LoadTextContent 和 LoadJsonContent 函数

这些功能是特定于二头肌的，因此构成了对抗 ARM 模板替代方案的游戏规则改变者。Bicep v0.4 引入了“loadTextContent”函数和“loadFileAsBase64”函数，而“loadJsonContent”从 0.7 版本开始提供。

它们是编译时函数，这意味着它们在编译阶段运行，这发生在开发人员的本地机器上(参见下面的更多细节)。指定为参数的文件内容将成为模板的一部分。它们需要编译时参数，因此您不能使用参数来指定哪个文件应该加载到 ARM 模板中。

这些功能最常见的用例是当资源需要 JSON、XML 或脚本文件作为其属性时，即 Azure 策略(可以采用要部署的 ARM 模板)、部署脚本(采用脚本)或 API 管理 API(可以采用 openAPI 规范)。虽然您可以使用原始字符串(以三撇号-' ' ' '开始和结束的字符串)，但通常最好在外部创作必要的文件，以突出编辑器语法或强制特定于语言的 linter 规则。

LoadTextContent 和 loadFileAsBase64 函数有一个严重的限制 ARM 模板中的字符串长度不能超过 131，072 个字符(9 KB 二进制文件大小)。直到 v0.7，一种常见的模式是在运行时使用包装在“json”函数中的 loadTextContent 将字符串转换为 JSON 对象。

如果加载的 JSON 文件没有缩小(去掉不必要的空格)，所有格式化字符，加上转义引号所需的字符，很快就会达到这个限制。Bicep 社区经常遇到这种限制，因此该团队决定实现一个名为“loadJsonContent”的函数——因为 ARM 模板本身就是一个 JSON，JSON 文件可以“原生”编译到模板中，并且不会考虑字符限制。

此外，loadJsonContent 允许用户选择 JSON 文件的特定部分，并基于外部的非 Bicep 文件动态构建模板。

然而，还有另一个限制——4mb 的最大模板大小。尽管 ARM 模板在部署前被缩小了，但在模板中包含文件时，我们仍然需要谨慎。

## Azure Bicep 部署流程

有时候，一旦你写了代码，你并不完全清楚在幕后发生了什么——你的 Bicep 文件应该产生真正的资源供你在 [Azure Clou](http://web.archive.org/web/20221206204748/https://www.netguru.com/blog/netguru-receives-gold-cloud-platform-competency-in-azure) d 中使用，但是错误和挫折可能会发生。

这个过程可能看起来很复杂，但是在部署过程中有三个重要方面可能会出现错误——开发人员应该了解这三个方面，以便更快、更有效地进行故障排除。

### 常见错误

当您编写 Bicep 代码时，遇到错误并不罕见。一些最常见的错误包括:

*   " BCP032:该值必须是编译时常量"
*   BCP120:此表达式正用于对[objectName]类型的属性[propertyName]的赋值，这需要一个可以在部署开始时计算的值
*   BCP177:此表达式正在 if-condition 表达式中使用，该表达式需要一个可以在部署开始时计算的值
*   BCP178:此表达式正在 for 表达式中使用，该表达式需要一个可以在部署开始时计算的值
*   BCP181:此表达式正用于函数[functionName]的参数中，该函数需要一个可以在部署开始时计算的值
*   BCP182:此表达式正用于变量[variableName]的 for-body 中，该变量需要可以在部署开始时计算的值

这些错误与你输入代码和 Azure 上提供的资源之间的流程是如何组织的有关。

### 部署流程阶段

Azure Bicep 部署流程的三个主要阶段是:在本地机器上进行代码编译，部署到 ARM API，以及由 ARM 运行时调用资源提供者 API。

本地机器阶段

![bicep blogpost-Page-1.drawio](img/d364310d6b4ca61c6587779ab57ee6ba.png)

第一步是将 Bicep 代码编译成 JSON-ARM 模板代码。您可以通过使用 Bicep build 命令手动完成这项工作，或者，如果您将 Bicep 文件指定为部署源，这将会自动完成。在这两种情况下，编译都发生在您的机器上。

### 当您使用 Bicep 模块时，在编译期间，它们包含在输出 ARM 模板中；因此，指定模块路径的值需要是编译时常数，这意味着它们在编译阶段必须是已知的。尽管如上所述，Azure CLI 工具会在部署阶段之前为您完成编译——它们是独立的进程，不能被视为一个进程。

ARM API 阶段

下一阶段是部署。Azure CLI 获取生成的 ARM 模板和参数文件，并将其上传到 Azure 资源管理器 API 进行处理。在那里，ARM API 首先运行一个模板验证——这包括检查模板的语法是否正确，所有参数的类型是否正确，以及是否提供了所需的类型。

### 如果无法正确处理模板，它将执行额外的内部过程以失败告终。在这个阶段开始之前可以确定的值是部署时间常数。

通常这是指传递给部署的参数。它们用于确定模板的外观——将要部署哪些资源(使用 if 语句时)和多少资源(使用循环时)。因为编译发生在部署之前，所以编译时常数也是部署时常数。

资源提供者 API 阶段

最后一个阶段——运行时——可以描述为遍历模板并执行对资源提供者 API 的 API 调用的过程。在这个阶段，当准备用于 API 调用的主体时，ARM 运行时可以从另一个资源属性或模块输出中获取一个值，并使用各种函数对其进行转换。

### 在运行时阶段开始之前，我们需要知道资源名称。需要注意的重要一点是，我们调用的每个模块都是一个部署——尽管它是由 ARM API 而不是用户触发的，但它仍然是一个部署，传递给模块定义的“参数”将成为被调用模块中的部署时间常数。

知道了这一点，我们可以利用模块将运行时值(即资源属性)转换为资源名称或位置、循环输入、if 条件等所需的部署时间常数。

使用 Azure Bicep 使用 Azure 策略创建多个用于收集日志的 Azure Web 应用的 9 个步骤

现在，让我们尝试使用我们获得的知识，部署 3 个资源组，每个资源组都有一个 Azure App Service Web Apps。另外。我们将编写一个 Azure 策略，确保所有 Web 应用程序都启用了诊断设置，并将日志写入日志分析工作区。

## 每个 WebApp 都需要一个 App 服务计划。WebApp 将基于 Linux-container，因此它需要一个 Azure 容器注册中心，该注册中心有权提取图像。我们将把它放在日志分析工作区和容器注册表的核心资源组中。该架构将如下所示:

Now let’s try to use the knowledge we gained and deploy 3 resource groups each with an Azure App Service Web Apps. Additionally. We will write an Azure Policy that will make sure that all Web Apps have Diagnostic Settings enabled and write logs to a Log Analytics Workspace.

Each WebApp requires an App Service Plan. WebApp will be Linux-container based, so it will require an Azure Container Registry with rights to pull images. We will place it in a core resource group for Log Analytics Workspace and Container Registry. The architecture will look like this:

我们还需要进行订阅级别的部署。因此，我们需要一个用于部署核心资源组的模块和一个用于项目资源组的模块。Azure 策略部署的代码将位于另一个 Bicep 文件中，我们将手动编译该文件，并将其作为 JSON 传递给策略资源。

![bicep blogpost-Page-2.drawio](img/186e035a67b0e79fdf83def9df401f3b.png)

第一步

第一步是创建一个针对订阅的顶级模块。在本模块中，我们将创建保存资源所需的资源组:

### 在第一行中，我们指定这个 Bicep 文件将订阅作为部署目标。

接下来，我们声明一个参数，将在其中创建 Azure 区域资源。

```
 targetScope = 'subscription'

param location string

resource coreResourceGroup 'Microsoft.Resources/resourceGroups@2021-04-01' = {
 name: 'core-rg'
 location: location
}

var projects = {
 project1: {
   webAppCount: 1
 }
 project2: {
   webAppCount: 3
 }
 project3: {
   webAppCount: 1
 }
}

resource projectResourceGroup 'Microsoft.Resources/resourceGroups@2021-04-01' = [for item in items(projects): {
 name: '${item.key}-rg'
 location: location
}] 
```

第 5-8 行声明了一个保存核心资源的资源组。

在第 10 行，我们声明了一个对象(实际上是一种字典),它将保存我们项目的资源组的配置。接下来是资源组声明，它将遍历对象中的项。资源组名称将取自密钥，后跟一个'-rg '后缀。

让我们将其保存为 main.bicep。现在，我们创建一个模块来部署 Log Analytics 工作区和容器注册表。

第二步

同样，我们首先需要为所有资源定义参数和位置。然后，我们为 Log Analytics 工作区的名称指定一个参数，并指定一个指示日志将保留多少天的整数值。

### 我们还指定了一个默认值 30 天，如果没有设置，将应用该值。在第 6-15 行，我们声明了日志分析工作区资源本身，在第 16 行，我们声明了模块的输出及其资源 ID。

```
 param location string

param logAnalyticsName string
param logRetentionInDays int = 30
resource logAnalyticsWorkspace 'Microsoft.OperationalInsights/workspaces@2021-06-01' = {
 name: logAnalyticsName
 location: location
 properties: {
   sku: {
     name: 'PerGB2018'
   }
   retentionInDays: logRetentionInDays
 }
}
output logAnalyticsId string = logAnalyticsWorkspace.id

param containerRegistryName string
resource containerRegistry 'Microsoft.ContainerRegistry/registries@2021-09-01' = {
 name: containerRegistryName
 location: location
 sku: {
   name: 'Basic'
 }
}
output containerRegistryId string = containerRegistry.id 
```

然后，我们开始讨论容器注册中心的规范。正如您所看到的，我们不需要在文件的顶部指定参数，但是我们可以在任何地方这样做，所以我们可以将特定于资源的参数放在资源声明的附近。

让我们将文件保存为 core.bicep。

第三步

现在我们需要回到 main.bicep 文件，在这里我们将声明在核心资源组中部署模块:

### 首先，我们从一些您可能会发现不寻常的事情开始——我们声明了一个变量' _dep '，它将保存当前的部署名称。正如我前面提到的，模块实际上是微软的资源。“资源/部署”类型，并且它们需要有一个名称。

为了避免与其他部署的任何潜在冲突，当我们在 Netguru 构建多级部署时，我们会在每个子部署前面加上一个父部署名称。但是，您需要小心，因为部署/模块名称有 64 个字符的限制。

```
 var _dep = deployment().name

module coreModule 'core.bicep' = {
 name: '${_dep}-core'
 scope: coreResourceGroup
 params: {
   location: location
   logAnalyticsName: 'system-log'
   containerRegistryName: 'mycompanyacr'
 }
} 
```

一旦我们有了帮助我们使用当前部署名称的变量，我们就定义一个模块并使用创建的文件作为源文件。我们使用字符串插值技术来指定名称。

我们需要定义模块接受的参数的值，并提供模块的范围——然后我们通过提供它的符号名将它设置为我们之前定义的资源组。我们可以在这里使用 resourceGroup 函数并提供资源组名，但是如果我们使用资源符号名，Bicep 编译器将知道我们首先需要资源，因此它将自动生成' dependsOn '子句，而不需要手动执行。

第四步

让我们用 WebApps 准备一个模块文件。除了 WebApp 本身，我们还需要一个应用服务计划来托管它。

### 同样，我们需要指定用于创建资源的参数。WebApp 名称必须是全局唯一的，因此我们使用 uniqueString 函数生成一个伪随机值，种子设置为资源组 ID。正如在参数中一样，我们声明我们需要多少个 web app，我们使用“range”函数创建一个整数列表，我们将迭代该列表以创建一系列 web app。

第五步

```
 param location string
param projectName string
param webAppCount int

resource appServicePlan 'Microsoft.Web/serverfarms@2020-12-01' = {
 name: 'plan-${projectName}'
 location: location
 properties: {
   reserved: true //true indicates that app service plan is linux based
 }
 sku: {
   name: 'B1'
 }
}

resource webApplication 'Microsoft.Web/sites@2018-11-01' = [for i in range(0, webAppCount): {
 name: 'webapp-${projectName}-${uniqueString(resourceGroup().id)}-${i}'
 location: location
 tags: {
   'hidden-related:${appServicePlan.id}': 'Resource'
 }
 properties: {
   serverFarmId: appServicePlan.id
 }
 identity: {
   type: 'SystemAssigned'
 }
}] 
```

现在我们需要用创建的 web 应用程序的身份来设置容器注册的权限。权限角色分配是一种扩展资源。因为 Container Registry 在一个单独的资源组中，所以我们需要使用一个模块来部署到不同的范围。因此，让我们将这个文件保存为 webapp.bicep，并打开一个新文件，为现有资源准备一个扩展资源:

### 该模块接受容器注册表名称和身份列表来授予 AcrPull 权限。因为容器注册中心已经部署，我们只需要一个对它的引用——所以我们使用' existing '关键字声明资源。

然后，我们再次遍历身份列表并构建角色分配资源。该名称必须是 GUID，因此我们必须基于权限和标识的名称生成一个 GUID。我们将范围设置为容器注册表(如果我们不这样做，角色将被分配给资源组)。

```
 param containerRegistryName string
param identities array

resource containerRegistry 'Microsoft.ContainerRegistry/registries@2021-09-01' existing = {
 name: containerRegistryName
}

resource roleAssignment 'Microsoft.Authorization/roleAssignments@2020-10-01-preview' = [for id in identities: {
 name: guid('acrPull', id)
 scope: containerRegistry
 properties: {
   roleDefinitionId: subscriptionResourceId('Microsoft.Authorization/roleDefinitions', '7f951dda-4ed3-4680-a7ca-43fe172d538d')
   principalId: id
   principalType: 'ServicePrincipal'
 }
}] 
```

内置角色定义有一个常量 GUID，我们可以安全地将它放在模板体中。要获取 GUID，我们可以运行 Azure CLI 命令“az role definition list-name ACR pull”。

让我们将其保存为 acrpull.bicep，并在 webapp.bicep 文件中声明使用它:

首先，我们需要获得 containerRegistry 资源 ID，以了解哪个 ACR 需要 WebApps 权限。Azure 资源 id 总是以相同的方式构造，所以我们使用“split”函数来获取正确设置范围和获取资源名称所需的特定部分:

```
 param containerRegistryId string
var _containerRegistryId = split(containerRegistryId, '/')
module acrpull 'acrpull.bicep' = {
 name: '${deployment().name}-acrpull'
 scope: resourceGroup(_containerRegistryId[2], _containerRegistryId[4])
 params: {
   containerRegistryName: _containerRegistryId[8]
   identities: [for i in range(0, webAppCount): webApplication[i].identity.principalId]
 }
} 
```

索引 2 下始终是订阅 GUID，4 表示资源组名，8 是资源名。

因为我们需要提供一组身份，所以我们再次在部署 WebApp 资源的相同范围内使用循环，并且我们引用每个身份来获得它们的托管身份的 principalId。

第六步

一旦我们编写了这个模块，我们需要声明它将被部署到我们创建的资源组中。我们回到 main.bicep，用循环定义模块:

这里的新内容是，对于语法，我们使用索引外观来引用资源组。Items 函数将总是以相同的顺序返回项目，因此我们可以确保我们将部署到正确的资源组。

```
 module webAppModules 'webapp.bicep' = [for (item, index) in items(projects): {
 name: '${_dep}-webapp-${index}'
 scope: projectResourceGroup[index]
 params: {
   location: location
   containerRegistryId: coreModule.outputs.containerRegistryId
   projectName: item.key
   webAppCount: item.value.webAppCount
 }
}] 
```

第七步

现在我们可以进入最后一部分，我们将编写一个由 Azure Policy 部署的模板来配置所有 WebApps 的诊断设置:

### DiagnosticSettings 资源也是一种扩展资源类型，因此我们需要有对该资源的引用。因为我们需要使用 WebApp 部署到 resourceGroup，所以我们只需要指定 WebApp 的名称。

现在，我们需要把这个做成一个 ARM 模板——让我们把它保存为 diagnostics.bicep，我们既可以使用 az bicep build-f diagnostics . bicep；或者，如果我们使用的是 Visual Studio 代码，我们可以右键单击该文件并选择“Build”选项。因此，我们的文件夹中会有一个 diagnostics.json 文件。

```
 param diagnosticSettingsName string
param logAnalyticsId string
param webAppName string

resource webApp 'Microsoft.Web/sites@2021-03-01' existing = {
 name: webAppName
}
resource webAppLogs 'Microsoft.Insights/diagnosticSettings@2021-05-01-preview' = {
 name: diagnosticSettingsName
 scope: webApp
 properties: {
   workspaceId: logAnalyticsId
   logAnalyticsDestinationType: 'Dedicated'
   logs: [
     {
       categoryGroup: 'allLogs'
       enabled: true
     }
     {
       categoryGroup: 'audit'
       enabled: true
     }
   ]
 }
} 
```

第八步

现在，让我们回到 main.bicep 文件并声明 Azure 策略定义，因为这种类型的资源需要在订阅级别上创建。我们还将为订阅分配定义，并允许托管服务标识具有策略定义中指定的角色。JSON 文件中有 Azure 策略是常见的做法，所以让我们创建一个并将其命名为“policy.json”:

### 接下来，我们需要使用 policy.json 和 diagnostics.json 文件创建一个策略定义资源。为此，我们利用“联合”功能，允许您组合多个对象:

请注意，我们使用 loadJsonContent 将策略文件和已编译的 ARM 模板与 diagnosticSettings 部署一起获取。这意味着我们不需要在这个文件中创作它，并且我们可以在创建部署模板时受益于 Visual Studio 代码插件。

```
 {
 "mode": "All",
 "policyRule": {
   "if": {
     "field": "type",
     "equals": "Microsoft.Web/sites"
   },
   "then": {
     "effect": "DeployIfNotExists",
     "details": {
       "roleDefinitionIds": [
         "/providers/microsoft.authorization/roleDefinitions/749f88d5-cbae-40b8-bcfc-e573ddc772fa",
         "/providers/microsoft.authorization/roleDefinitions/92aaf0da-9dab-42b6-94a3-d43ce8d16293"
       ],
       "type": "Microsoft.Insights/diagnosticSettings",
       "name": "[parameters('profileName')]",
       "existenceCondition": {
         "allOf": [
           {
             "count": {
               "field": "Microsoft.Insights/diagnosticSettings/logs[*]",
               "where": {
                 "allOf": [
                   {
                     "field": "Microsoft.Insights/diagnosticSettings/logs[*].enabled",
                     "equals": false
                   }
                 ]
               }
             },
             "equals": 0
           }
         ]
       },
       "deployment": {
         "properties": {
           "mode": "incremental",
           "parameters": {
             "diagnosticSettingsName": {
               "value": "[parameters('profileName')]"
             },
             "logAnalyticsId": {
               "value": "[parameters('logAnalyticsId')]"
             },
             "webAppName": {
               "value": "[field('name')]"
             }
           }
         }
       }
     }
   }
 },
 "parameters": {
   "logAnalyticsId": {
     "type": "String"
   },
   "profileName": {
     "type": "String",
     "defaultValue": "logs"
   }
 }
} 
```

此外，我们从策略定义文件中提取了必要的角色，并使用它们为托管服务身份分配权限。

```
 resource policyDefinitionLogs 'Microsoft.Authorization/policyDefinitions@2020-09-01' = {
 name: 'policy-logs-webapp'
 properties: union(
   {
     displayName: 'App Service Web App Diagnostic Settings'
     policyType: 'Custom'
     description: 'This policy definition will enable logging of App Service Web App into Azure Log Analytics'
     metadata: {
       category: 'Logging'
     }
   },
   loadJsonContent('policy.json'),
   { policyRule: { then: { details: { deployment: { properties: { template: loadJsonContent('diagnostics.json') } } } } } }
 )
}

resource policyAssignment 'Microsoft.Authorization/policyAssignments@2020-09-01' = {
 name: 'policy-logs-webapp'
 location: location
 identity: {
   type: 'SystemAssigned'
 }
 properties: {
   displayName: 'WebApp Diagnostic Settings'
   enforcementMode: 'Default'
   policyDefinitionId: policyDefinitionLogs.id
   parameters: {
     logAnalyticsId: {
       value: coreModule.outputs.logAnalyticsId
     }
   }
 }
}

resource policyRoleAssignments 'Microsoft.Authorization/roleAssignments@2020-10-01-preview' = [for role in loadJsonContent('policy.json', '$.policyRule.then.details.roleDefinitionIds'): {
 name: guid('policyAssignment', toLower(role))
 properties: {
   roleDefinitionId: '${subscription().id}${role}'
   principalId: policyAssignment.identity.principalId
   principalType: 'ServicePrincipal'
 }
}] 
```

第九步

完成后，我们可以使用 Azure CLI 运行我们的部署:

### az 部署 sub create-n web apps-l westeurope-f main . bicep-p location = westeurope

部署完成后，我们可以转到 Azure Portal 中的订阅策略刀片，并看到我们有一个策略分配，因为我们的 WebApps 不兼容。

要解决这个问题，我们需要创建一个补救任务，也是使用 CLI:

az 策略补救创建-名称 webapp-logs -资源-发现-模式重新评估合规性-策略-分配策略-logs-webapp

在任务运行之后，我们可以看到我们从 JSON 文件提供的部署已经在带有 WebApps 的资源组上运行了。

由于我们的策略现已到位，我们在订阅中使用此策略创建的任何其他 web 应用程序将在几分钟后自动接收相同的诊断设置，这是策略评估开始的时间。

开始将 Azure Bicep 用于您的基础设施即代码

一旦实现，Azure Bicep 提供了比 ARM 模板更加有效和直观的基础设施即代码方法。开发人员越来越多地转向这种开源选项，以在项目开发生命周期的每个阶段简化他们的基础设施部署。

## 随着快速建立的开发人员在线社区、微软的支持以及更简单、更容易的编码体验，越来越多的开发人员转向 Azure 上的 Bicep。为了跟上流行的开发进度，并确保您没有使用过时的概念，请查看我们关于 Azure 开发的[概述。您一定会找到一个解决方案来推动您的开发团队向前发展！](http://web.archive.org/web/20221206204748/https://www.netguru.com/services/azure-development)

Once implemented, Azure Bicep provides a much more effective and intuitive infrastructure-as-code method than ARM templates. Developers are increasingly turning to this OpenSource option to streamline their infrastructure deployment at every stage of their project development lifecycle.

With a fast-building online community of developers, the support of Microsoft, and a simpler, easier coding experience, more and more developers are turning to Bicep on Azure. To keep up with popular development progress, and ensure you’re not using out-dated concepts, take a look at our [overview of development on Azure](http://web.archive.org/web/20221206204748/https://www.netguru.com/services/azure-development). You’ll be sure to find a solution to propel your development team forwards!