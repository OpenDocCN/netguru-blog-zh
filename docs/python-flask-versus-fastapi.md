# Python Flask 与 FastAPI:你该选哪个？

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/python-flask-versus-fastapi>

 Flask 和 FastAPI 是流行的 Python 微框架，用于构建基于数据科学和机器学习的小规模网站或应用程序。

尽管 FastAPI 是一个年轻的框架，但是越来越多的开发者正在他们的新项目中使用它。只是炒作还是 FastAPI 优于 Flask？我们准备了 Flask 和 FastAPI 的主要优缺点对比，以帮助您决定下一个数据科学应用的最佳选择。

## 什么是 FastAPI？

FastAPI 是一个针对 [Python 3.6 及更新版本的 web 开发框架。](/web/20221229081739/https://www.netguru.com/glossary/python)它于 2018 年发布，是一个基于 [Starlette](http://web.archive.org/web/20221229081739/https://www.starlette.io/) 的开源框架，使用标准 Python 类型提示。该框架主要用于构建快速 web 应用程序和 Rest APIs。

FastAPI 构建在异步服务器网关接口(ASGI) web 服务器[uvicon](http://web.archive.org/web/20221229081739/https://www.uvicorn.org/)之上，但是你也可以使用相关的中间件挂载 web 服务器网关接口(WSGI)应用。

它目前被优步，微软，爆炸人工智能和其他公司使用。

## FastAPI 的优点

FAstAPI 具有很高的性能，可以很容易地支持并发，并且它提供了一个简单易用的依赖注入系统。内置数据验证是需要考虑的另一个好处。

### 完美的表现

如果我们要说出 FastAPI 击败 Flask 的一个品质，那就是性能。FastAPI 实际上被称为最快的 Python web 框架之一。事实上，只有构建 FastAPI 的 Starlette 和 Uvicorn 更快。这种卓越的性能正是由 ASGI 实现的，得益于 ASGI，FastAPI 支持并发和异步代码。这是通过用异步定义语法声明端点来实现的。

### 本机并发支持

在 Python 中实现并发编程曾经非常困难 Python 3.4 中添加了异步 I/O。使用 FastAPI，可以轻松实现并发性，而无需担心事件循环或异步/等待管理。

开发人员只需通过 async def 函数将第一个路径函数声明为协同程序，然后通过 await 声明特定的点 aws。

### 依赖注入支持

FastAPI 提供了一个简单易用的依赖注入系统。[依赖注入](/web/20221229081739/https://www.netguru.com/blog/dependency-injection-with-python-make-it-easy)，一种声明代码正确运行所需的必要组件的组合方式。

这是一种实现控制反转的方法，它增加了代码的模块化，使系统更具可伸缩性。在 FastAPI 中，开发人员可以简单地在分配给 API 端点的路径操作函数中声明相关的依赖关系。

### 内置文档支持

FastAPI 提供了一个非常方便的自动文档系统。它提供了一个基于浏览器的用户界面，交互地记录 API，由 Swagger UI [GUI](/web/20221229081739/https://www.netguru.com/blog/building-cross-platform-gui-applications-in-kivy) 提供支持。

或者，开发人员可以简单地输入/redoc 来获得包含所有列出的端点的替代文档。文档将总是允许开发者容易地向其他人解释程序，使前端工程师更容易使用你的后端，并在测试 API 端点时增加便利。

### 内置数据验证

这是一个巨大的好处——内置的数据验证允许开发人员通过跳过验证来创建紧凑的代码。它可以在运行期间检测无效的数据类型，并以 JSON 格式返回错误输入的原因。

FastAPI 为此使用了 [Pydantic](http://web.archive.org/web/20221229081739/https://pydantic-docs.helpmanual.io/) 库，这极大地简化了验证过程，并确保了比手工输入更快的速度。它还可以减少错误，FastAPI 作者声称[它可以减少开发人员多达 40%的错误。](http://web.archive.org/web/20221229081739/https://fastapi.tiangolo.com/)

## FastAPI 的缺点

你应该考虑到缺乏内置的安全系统和开发人员的小社区。

### 缺乏内置安全系统

FastAPI 不提供内置的安全系统。相反，它为安全机制提供了一个 fastapi.security 模块。同时，它确实支持 OAuth2.0。

### 开发人员的小社区

FastAPI 相对较新(比 Flask 小 8 岁)，这意味着它的社区仍然相当小，并且他们可用于该框架的教育材料仍然有限。搜索后，你会发现只有少数书籍，指南或教程可用。与此同时，它越来越受欢迎，所以这可能会在未来几年发生变化。

## 烧瓶是什么？

Flask 是一个为 Python 编写的微型 web 框架。它是轻量级的、开源的，提供了一个小而易扩展的核心。它主要用于开发极简的 web 应用程序和 Rest APIs。

Flask 于 2010 年发布，作为基于 Werkzeug 和 Jinja2 的框架。它建立在 Web 服务器网关接口(WSGI)之上。您可以在 Flask 中使用 ASGI 服务器，但是您需要利用 WSGI 到 ASGI 中间件。

该框架通过扩展支持 REST 开发，例如 Flask-RESTful、Flask-class、Flask-RESTPlus 等。它非常适合构建电子商务系统、社交媒体机器人或静态网站。不适合高负载的企业软件。

Flask 目前由网飞、Lyft 或 Zillow 使用。它被认为是最受初学者欢迎的 Python 开发框架。

## 烧瓶的优点

使用 Flask，您可以构建易于扩展的应用程序。另一个优点是烧瓶具有很大的灵活性。

### 非常适合构建可扩展的解决方案

Flask 的建立是为了让科技项目快速发展。Flask 应用程序很容易扩展。该框架支持创建复杂的应用程序，允许开发人员根据需要轻松添加新的功能和用例。因此，Flask 是小型企业的绝佳选择，这些企业打算在未来几年发展他们的新解决方案。

### 可用的充足资源

Flask 已经使用了十多年，所以它有大量的支持资源。有很多可用的扩展，Flask 社区也建立得很好——你可以查看 [GitHub](http://web.archive.org/web/20221229081739/https://github.com/humiaozuzu/awesome-flask) 获得一些有用的数据。这使得自学者相对容易学会。

### 极大的灵活性

开发人员肯定会喜欢 Flask 的大部分可以更改，这在大多数其他开发框架中是很少见的。这是 Flask 最大的优点之一。由于这种灵活性和极简主义的本质，你可以很容易地改变你的项目的过程，同时保持整体结构的稳定。Fast API 在代码和布局方面也很灵活，但是 Flask 更加灵活。

### 安全性

Flask 可能是一个极简框架，但它能够很好地处理常见的安全问题，如 CSRF、XSS 或 JSON 安全。开发者也可以从第三方扩展中受益，比如 [Flask-Security](http://web.archive.org/web/20221229081739/https://pythonhosted.org/Flask-Security/) ，但是他们应该意识到添加这些扩展会导致性能下降。但是，开发人员必须始终仔细评估这些扩展，并记住定期手动更新它们，以及在发现漏洞时进行手动更新。

## 烧瓶的缺点

考虑到缺乏对异步的支持，并且 Flask 使用了第三方模块，这可能会对安全性产生负面影响。

### 缺乏对异步的支持

Flask 是为像 Gunicorn 这样的 WSGI 服务开发的，所以它不提供本地异步支持。这意味着长时间运行的查询实际上可能会阻塞应用程序。用 Flask 构建的 REST API 将能够处理更少的用户。总而言之，每个请求都会被轮流处理，这需要更多的时间。

### 模块

Flask 使用第三方模块，这增加了安全漏洞的风险。应用模块意味着开发过程不仅仅是开发者和框架之间的事情。如果实现了一个恶意的模块，后果会很严重，所以程序员必须格外注意安全机制。

### 缺乏数据验证

Flask 中没有数据格式的验证，这意味着开发人员可以传递任何类型的数据，包括字符串或整数。有一些扩展可以弥补这个缺点，例如 Flask-Marshmallow 或 Flask-inputs。或者，开发人员可以为数据输入添加验证脚本，但这需要额外的工作。

## Flask 和 FastAPI:基于数据科学和机器学习构建网站或应用程序时选择哪个

Flask 和 FastAPI 都可以用 Python 快速设置 web 服务器和[数据科学应用](/web/20221229081739/https://www.netguru.com/blog/python-versus-scala)。在部署时，它们需要同样多的努力。如何决定哪个框架更适合你的下一个项目？

当速度和性能至关重要时，FastAPI 是最佳选择。如果您正在构建自己的 CDN，并期望获得巨大的流量，请选择这个更新的框架。有了 FastAPI，你只需下载并使用已经建立在尖端技术上的框架，从项目模板中获益将帮助你节省时间。

当您构建 API 时，FastAPI 将是比 Flask 更好的选择，尤其是当考虑到微服务时。在这种情况下，选择 Flask 的唯一理由是，如果您的组织已经有许多围绕该框架构建的工具。

相比之下，当您需要构建一个简单的带有几个 API 端点的微服务时，请选择 Flask。这也是[建立机器学习模型和数据科学支持的网络应用原型](/web/20221229081739/https://www.netguru.com/services/python-development)的一个很好的选择。

此外，如果你想开发一个小规模的应用，但有潜力快速增长，并朝着你尚未完全确定的方向发展，那么 Flask 是一个理想的选择。它使用简单，运行流畅，因为只有少量的依赖项，即使在您继续扩展时也是如此。