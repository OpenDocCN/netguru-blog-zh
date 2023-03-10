# Python 的框架比较:Django、Pyramid、Flask、Sanic、Tornado、BottlePy 等等

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/python-frameworks-comparison>

 Python 框架可以分为几个领域，因为 Python 是一种非常多样化的语言，可以用于各个领域。这些领域都有自己的框架，其中一些比另一些更受欢迎。Python 应用的最热门的领域之一[是 web 开发，我们今天将重点介绍的](/web/20221004125922/https://www.netguru.com/services/python-development)。

所展示的框架可以分为三类:全栈框架，它为服务器和客户端提供了许多开箱即用的特性；微框架，它提供服务器端支持(有时，它们可以扩展到客户端),允许只使用一个 [Python 文件创建 web 应用程序；最后，异步框架，异步处理请求](/web/20221004125922/https://www.netguru.com/blog/python-pros-and-cons)。

## 全栈 web 框架

全栈 web 框架是一个全面的解决方案，由不同的库配置组成，可以毫不费力地协同工作。全栈 web 框架可以支持开发人员构建后端服务、前端接口和数据库。

### 姜戈

Django 是最流行的 Python 框架之一。它提供了许多开箱即用的功能，如管理面板或通用视图和表单。Django 的主要特点是:

*   一个管理脚本(“manage.py”)，可用于执行大多数特定于框架的操作(如启动开发服务器、创建管理员用户、收集静态文件等)。),

*   同步请求处理，

*   MTV(模型-模板-视图)架构模式(它是模型-视图-控制器模式的变体)，

*   用于与数据库通信的自定义对象关系映射(ORM ),

*   视图上下文创建和动作处理的函数和类的使用，

*   Django 很严格，并把自己的编码风格强加给开发人员——大量的元编程，

*   非常好，大量的文档和例子，

*   自定义 HTML 模板渲染引擎，

*   自定义 URL 路由系统，

*   符合 WSGI 标准，

*   支持静态文件- URL 路由以及检测和收集，

*   大量的外部模块，例如 Django REST 框架、Django [CMS](/web/20221004125922/https://www.netguru.com/blog/python-cms) 、Django 通道( [websockets](http://web.archive.org/web/20221004125922/https://www.netguru.com/blog/how-we-built-ember-socket-guru-a-websockets-integration-case-study) )。

Django 非常适合较大的项目，在这些项目中需要大量的后端和前端支持，或者在时间起着关键作用的情况下，因为 Django 提供了大量现成的组件。Django 中的编码主要依赖于定制代码的通用部分。开发者必须遵循给定元素的一组规则。对于需要大量代码灵活性的项目，Django 可能不是最佳选择。

37514[Github](http://web.archive.org/web/20221004125922/https://github.com/django/django)stars/183588[stack overflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/django)提问

![5-Sep-30-2020-01-29-47-33-PM](img/f104958f9e6c4432146cd39000af4d8c.png)

* * *

Web2py 侧重于安全性、开发速度和易用性。它提供了许多开箱即用的特性:web 服务器、数据库、管理面板、wiki 或网格小部件。框架的主要特点是:

*   同步请求处理，

*   充当 ORM 的自定义数据库抽象层(DAL ),

*   强制使用 MVC 结构，

*   函数和类可用于创建控制器，

*   严格的“做事方式应该只有一种”哲学，

*   包含大量示例的丰富文档，

*   允许在模板中使用 Python 代码的自定义 HTML 引擎，

*   自定义路由 URL 函数，为操作和静态文件生成内部路径，

*   支持 WSGI 标准，但也可以使用 CGI(通用网关接口)、FastCGI、GAE(谷歌应用引擎)或其他，

*   在开发过程中提供静态文件路由和流，

*   具有内置的 REST 服务，但需要 Tornado 框架来使用 Web Socket。

Web2py 受到了 Ruby on Rails 和 T2 Django 框架的极大启发，并从两者中吸取了精华。
对于想从 Ruby 迁移的程序员，或者厌倦了 Django 但正在寻找另一个大的功能丰富的框架的人来说，这是一个不错的选择。
它提供了一个“管理”应用程序，作为基于 web 的应用程序开发和管理 IDE(例如应用程序创建、代码编辑器)。它也得到 PyCharm 的支持。总的来说，Web2Py 并不缺少 Django 拥有的任何功能。这两个框架可以用来完成相同的任务。Web2Py 更年轻，它的社区比 Django 更小，所以在遇到麻烦时寻求帮助可能会更难一些。

1665[GitHub](http://web.archive.org/web/20221004125922/https://github.com/web2py/web2py)stars/2004[stack overflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/web2py)问题

![0Python’s Frameworks Comparison: Django, Pyramid, Flask, Sanic, Tornado, BottlePy and More](img/fa7e8996218325884271b86c5bb1420c.png)

* * *

TurboGears 连接了许多外部服务来创建一个功能框架:

*   同步请求处理，

*   模型-视图-控制器(MVC)模式，

*   使用 SQLAlchemy ORM，

*   允许使用函数和类视图上下文生成，

*   它提供了一些现成的泛型类(非常有趣的用于 REST API 创建的 APIController)，

*   文档有点混乱，但这可能是一个习惯的问题，

*   使用 Kajiki 模板语言，

*   自定义 URL 路由/调度方法，

*   符合 WSGI 标准，

*   支持静态文件路径配置

*   可以使用附加模块进行扩展，例如用于 Web Sockets 支持的 Circus 和 Chaussette。

这个框架不像它的前两个版本那样流行，但是仍然值得一试。

259[GitHub](http://web.archive.org/web/20221004125922/https://github.com/TurboGears/tg2)stars/107[stack overflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/turbogears)提问

![Python’s Frameworks Comparison: Django, Pyramid, Flask, Sanic, Tornado, BottlePy and More](img/255ffe9bd752a0ee49ba3603b893a54c.png)

微框架

* * *

## Python 微框架不需要特殊的工具或库。例如，他们可能没有来自验证的数据库抽象层。微框架可能更适合小型应用程序。

它是最流行的 Python 微框架之一，既可靠又快速。据说它是作为一个笑话被创造出来的。该框架的主要特点是:

同步请求支持，

*   不强制任何项目架构，但有一些建议(包、模块、蓝图)，

*   它不提供 ORM，但可以使用 SQLAlchemy 或其他，

*   支持函数以及一些类似 Django 的泛型类视图(从 Flask 0.7 开始)，

*   松散的编码风格，它不强制任何解决方案，大部分决策留给开发人员自行决定，

*   有例子的好文档，

*   可以使用 Jinja2 HTML 模板引擎，

*   布线系统工具，

*   符合 WSGI 标准，

*   支持基本的静态文件路由，

*   可以使用一些额外的第三方模块进行扩展，例如用于 REST API 创建的 Flask-RESTful 或用于 Web Sockets 支持的 flask-socketio。

*   这个框架将在中小型项目中发挥作用。它有一些现成的第三方模块和优秀的本地解决方案。Flask 应该在需要复杂定制功能的工作中证明自己，但 Django 似乎太大了。另一方面，从一开始就为一个更大的项目设置 Flask 可能会很棘手，因为没有这样做的“官方”方法。

39946[Github](http://web.archive.org/web/20221004125922/https://github.com/pallets/flask)stars/24512[stack overflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/flask)问题

39,946 [Github](http://web.archive.org/web/20221004125922/https://github.com/pallets/flask) stars / 24,512 [StackOverflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/flask) questions

![Python’s Frameworks Comparison: Django, Pyramid, Flask, Sanic, Tornado, BottlePy and More](img/592337ac05e5421347640126f3e4fb37.png)

金字塔从最小安装开始，需要时可以扩展。值得注意的是，它是集成了网络相关技术的[塔](http://web.archive.org/web/20221004125922/https://pylonsproject.org/)项目的一部分。
这些是金字塔最重要的特征:

* * *

提供同步请求处理，

*   视图上下文可以用函数和类来定义，

*   没有特定的 ORM，但是建议使用 SQLAlchemy，

*   不强制任何编码风格或项目架构——[TIMTOWTDI](http://web.archive.org/web/20221004125922/https://docs.pylonsproject.org/projects/pyramid/en/1.10-branch/designdefense.html?highlight=MVC)，

*   提供了包含教程和示例的优秀文档，

*   没有提供特定的 HTML 模板引擎，但是推荐使用[变色龙](http://web.archive.org/web/20221004125922/https://docs.pylonsproject.org/projects/pyramid-chameleon/en/latest/)，

*   一个有趣的定制路由系统允许多个视图匹配一个 URL，

*   它符合 WSGI，

*   广泛的静态文件支持–文件服务、静态文件的 URL 路由，

*   可以使用外部模块进行扩展，例如用于 REST APIs 的 [Cornice](http://web.archive.org/web/20221004125922/https://cornice.readthedocs.io/en/latest/) ，具有异步支持的 [aiopyramid](http://web.archive.org/web/20221004125922/https://github.com/housleyjk/aiopyramid) 。

*   如果您不想花时间学习定制框架解决方案(如 ORM ),但仍然需要一个广泛的工具来构建软件，这个框架可能是一个不错的选择，因为 Pyramid 支持使用许多著名的独立解决方案。它具有良好的扩展能力——宣传自己是一个“从小处着手，大处着手”的框架。

2974 个 [GitHub](http://web.archive.org/web/20221004125922/https://github.com/Pylons/pyramid) 问题/2060 个 [StackOverflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/pyramid) 问题

2,974 [GitHub](http://web.archive.org/web/20221004125922/https://github.com/Pylons/pyramid) questions / 2,060 [StackOverflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/pyramid) questions

![Python’s Frameworks Comparison: Django, Pyramid, Flask, Sanic, Tornado, BottlePy and More](img/2aa696d22e40a595180bf0d099717991.png)

允许以与面向对象 Python 项目相同的方式创建 web 应用程序。它没有全栈功能，也不强制任何特定的解决方案——开发者可以决定如何解决开发过程中出现的问题。CherryPy 的特点是:

* * *

提供同步请求支持，

*   不强制任何项目结构或架构，

*   不提供任何 ORM，但是可以使用 SQLAlchemy 或 SQLObject，

*   不提供任何 HTML 模板引擎，

*   具有松散的编码风格，

*   提供像样的文件，

*   可以使用路由系统 [routes](http://web.archive.org/web/20221004125922/https://routes.readthedocs.io/en/latest/) (Python 版本的 [Rails](http://web.archive.org/web/20221004125922/https://www.netguru.com/ruby-on-rails-perfect-way-to-kickstart-your-software-business) 路由系统)，

*   符合 WSGI，

*   对静态文件有很好的支持——允许服务文件或整个文件目录，

*   只允许使用内置工具创建 REST APIs，

*   通过 [ws4py](http://web.archive.org/web/20221004125922/https://ws4py.readthedocs.io/en/latest/) 模块促进 Web 套接字的使用。

*   CherryPy 的主要优势是它附带了一个生产就绪的 WSGI 服务器，这就消除了在部署期间设置外部服务器的必要性。这个框架的主要缺点是它不太受欢迎，因此它的外部模块数量较少，社区也不够活跃。

829[GitHub](http://web.archive.org/web/20221004125922/https://github.com/cherrypy/cherrypy)stars/1244[stack overflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/cherrypy)提问

829 [GitHub](http://web.archive.org/web/20221004125922/https://github.com/cherrypy/cherrypy) stars/ 1,244 [StackOverflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/cherrypy) questions

![Python’s Frameworks Comparison: Django, Pyramid, Flask, Sanic, Tornado, BottlePy and More](img/3f47a5f1f46130e72361003546a6465e.png)

这是另一个标榜自己快速简单的微框架。值得注意的是，BottlePy 是作为一个单独的模块交付的，没有额外的依赖。功能:

* * *

实现同步请求处理，

*   提供自定义 HTML 引擎，但也可以使用其他引擎，如樱井真子、Jinja2 或 Cheetah。

*   不提供 ORM，但可以使用外部解决方案，例如 SQLAlchemy 或 Macaron，

*   不强制任何项目架构，

*   提供足够的文件，

*   有一个定制的路由系统，但可以使用 Werkzeug 路由系统(通过 [bottle-werkzeug](http://web.archive.org/web/20221004125922/https://pypi.org/project/bottle-werkzeug/) )，

*   实现 WSGI 标准，

*   提供基本的静态文件路由，

*   提供 greenlets(带有 [gevent](http://web.archive.org/web/20221004125922/http://www.gevent.org/) )作为异步请求处理解决方案，

*   没有外部模块也可以创建 REST API 支持 JSON 客户端数据。

*   由于它很小(只有一个文件)并且不需要外部依赖(只有 Python 标准库)，对于想要开始学习 web 开发的初学者来说，它是一个不错的选择。对于非常小的站点或一次性测试，它也可能做得很好。BottlePy 不会是中型或大型项目的最佳选择，因为它需要一些工作才能达到较重框架的起点。

5795 个[GitHub](http://web.archive.org/web/20221004125922/https://github.com/bottlepy/bottle)stars/1288 个 [StackOverflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/bottle) 问题

异步框架

* * *

## 异步框架意味着一种并行编程的形式，它使工作的一个组件能够独立于主应用程序线程运行。工作完成后，主线程会得到通知。

这是一个广泛的异步 Python 框架。它提供了一个具有中间件、信号、正常关机等功能的服务器:

异步请求处理、客户端和服务器 websockets，

*   可以使用 [GINO](http://web.archive.org/web/20221004125922/https://github.com/fantix/gino) 异步 ORM，

*   支持基于函数和类的视图，

*   足够的文档，但有点难以浏览，

*   可以使用纯 SQLAlchemy，但是推荐使用 GINO 作为异步包装器，

*   支持 postgres、MySQL、Redis 异步驱动程序、

*   没有现成的模板引擎，但是可以应用 Jinja2 或樱井真子，

*   定制路由系统，

*   没有 WSGI 支持，

*   支持静态文件的路由，

*   许多第三方模块可以进一步扩展框架，例如用于 REST API 创建的 [aiohttp-apispec](http://web.archive.org/web/20221004125922/https://aiohttp-apispec.readthedocs.io/en/latest/) ，用于用户认证和权限的 [aiohttp-security](http://web.archive.org/web/20221004125922/https://github.com/aio-libs/aiohttp-security) 。

*   由于 Aiohttp 提供了许多现成的特性(例如，对客户端和服务器端、websockets、中间件、信号的支持)，因此它可以用于中等规模的项目(甚至更大的项目)。

6378[Github](http://web.archive.org/web/20221004125922/https://github.com/aio-libs/aiohttp)stars/503[stack overflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/aiohttp)提问

6,378 [Github](http://web.archive.org/web/20221004125922/https://github.com/aio-libs/aiohttp) stars / 503 [StackOverflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/aiohttp) questions

![Python’s Frameworks Comparison: Django, Pyramid, Flask, Sanic, Tornado, BottlePy and More](img/255ffe9bd752a0ee49ba3603b893a54c.png)

Sanic 是一个非常类似烧瓶的框架:它很小，很自由，为开发人员留下了很多空间。它的主要特征是速度。以下是一些其他特征:

* * *

支持异步请求处理程序，

*   不提供任何数据库接口，但是可以安装 [GINO](http://web.archive.org/web/20221004125922/https://python-gino.readthedocs.io/en/latest/) 异步 ORM，

*   提供函数和类作为视图上下文的来源，

*   编码风格相当松散，非常类似于 Flask，

*   与“阅读文档”一起交付的文档，

*   Jinja2 HTML 模板引擎可以使用，

*   定制路由系统，

*   默认情况下不符合 WSGI，但是可以安装第三方模块( [sanic-dispatcher](http://web.archive.org/web/20221004125922/https://github.com/ashleysommer/sanic-dispatcher) )来支持它，

*   具有基本的静态文件路由，

*   可以使用附加模块进行扩展，例如用于 REST API 创建的 [Sanic CRUD](http://web.archive.org/web/20221004125922/https://github.com/Typhon66/sanic_crud) 。

*   当您已经有了一些使用 Flask 的经验时，选择 Sanic 应该是一个不错的决定，因为这两个框架有很多共同点。
    Sanic 提供默认配置处理，而在前面提到的 [aiohttp](http://web.archive.org/web/20221004125922/https://aiohttp.readthedocs.io/) 中，用户需要自己完成。它有一些有趣的第三方模块，如请求速率限制器或 graph QL T4 集成。

10625[Github](http://web.archive.org/web/20221004125922/https://github.com/huge-success/sanic)stars/58[stack overflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/sanic)提问

Tornado 是一个 Python web 框架和异步网络库，最初开发于 FriendFeed(一个社交聚合网站)。由于这一点，它提供了与谷歌、脸书和 Twitter 等社交服务的内置集成。与其他框架和库的集成也是可能的:Twisted、asyncio 甚至 WSGI 应用程序。龙卷风的特征:

* * *

提供了许多可用于创建应用程序的通用类，例如路由器，或用于 websockets 的 SocketHandler，

*   自定义 HTML 模板引擎，

*   清晰易读的文档，

*   函数和类可用于定义动作和处理请求，

*   自定义路由处理–提供可用于路由创建的通用类，

*   它支持 WSGI，但不推荐——用户应该使用 Tornado 自己的接口，

*   开箱即用的 websockets 支持、认证(例如通过 Google)和安全特性(例如 cookie 签名或 XSRF 保护)，

*   创建 REST API 不需要额外的工具。

*   在有大量可以快速处理的输入连接的情况下，或者在实时解决方案(例如聊天)中，该框架应该工作良好。
    Tornado 试图解决 [c10k 问题](http://web.archive.org/web/20221004125922/https://en.wikipedia.org/wiki/C10k_problem)所以高处理速度是当务之急。
    Tornado 的另一个优势是其对社会服务的原生支持。这个框架对于创建标准的 CRUD 站点或者[大型商业应用](/web/20221004125922/https://www.netguru.com/services/enterprise-mobile-app-development)来说不是一个好的选择，因为它不是被设计成那样使用的。对于更大的项目，它可以作为更大结构的一部分与 WSGI 应用程序集成，并处理需要高处理速度的任务。

16768[Github](http://web.archive.org/web/20221004125922/https://github.com/tornadoweb/tornado)stars/3263[stack overflow](http://web.archive.org/web/20221004125922/https://stackoverflow.com/questions/tagged/tornado)问题

web 开发最好的 Python 框架是什么？

### 如果需要服务器和浏览器端的广泛支持，那么全栈框架可能是一个不错的选择。

The web frameworks presented above are merely a small chunk of a bigger and broader family of Python frameworks.
Django, Pyramid, Flask, Sanic, Tornado, BottlePy and other Python frameworks have their strong and weak points and, as with everything else, there is no perfect match that will solve every given task.
The most important questions that one has to answer when choosing the framework are dictated by problems that need to be solved.

*   对于较小的项目或者优先考虑代码编写灵活性的地方，微框架可能是一个不错的选择。
*   在请求处理速度扮演重要角色或者项目必须处理长响应时间的情况下，异步框架应该可以解决这个问题。
*   In cases where request processing speed plays an important role or a project will have to deal with long response times, asynchronous framework should do the trick.