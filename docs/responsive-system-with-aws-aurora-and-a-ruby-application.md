# 带有 AWS Aurora 和 Ruby 应用程序的响应系统

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/responsive-system-with-aws-aurora-and-a-ruby-application>

 在这篇文章中，我将带您了解我们如何通过 AWS Aurora 服务满足客户对更高网络流量的需求，并逐步将其融入现有的基础设施设置。

## 首先要做的事情...

在我进入将我们的基础设施提升到下一个级别的实际计划之前，让我简单描述一下背景和我们所处的位置。为了简单起见，我将跳过所有不相关的系统部分，如缓存、队列或应用层，它们与升级无关。

也就是说，这个系统非常简单。构成该架构的组件有:

*   RDS PostgreSQL；
*   为 Ruby on Rails (RoR)应用服务的静态 ec2 车队；
*   应用负载均衡器；
*   cloud watch；

继名句 *“一图胜过千言”，* 下图代表系统的原始设计。

![AWS Aurora RDS](img/56fa7b0ad9072edba69e0ed10a1826de.png)

如你所见，它是一个非常流行的 [三层架构](http://web.archive.org/web/20221007194155/https://en.wikipedia.org/wiki/Multitier_architecture#Three-tier_architecture) ，表示层作为客户端的移动 app，应用层作为我们的 RoR 应用，数据层使用和 RDS PostgreSQL 数据库。这样的设计非常适合我们的应用程序，但是传入的流量受限于 EC2 机器和 RDS 的计算能力。

## 挑战

随着数字市场的增长，每个人，尤其是在冠状病毒季节，都疯狂地使用互联网服务，客户决定推广他们的服务并吸引更多用户，这显然会显著增加应用程序端的流量。需求很明确——“*处理 2.5k 同时活跃用户的峰值，而没有任何服务中断* ”。这就是我们所做的。

## 这个计划

我们必须根据需求规划新的基础设施，但是首先我们对当前设置进行了性能测试，以获得参考点并检查实际瓶颈。

第一次测试显示了两件事:

1.  当前的设置可以处理 4.5k 个请求/分钟，这与我们想要达到的目标相差甚远。
2.  几分钟后，负载平衡器上开始出现超时(504 HTTP 错误)

![AWS](img/be9af6ad7241a6ee24e1f4e8d719f718.png)

显然，少量的应用 EC2s 是一个瓶颈，导致我们无法在 10 秒内完成客户端请求。有了这些信息，下一步就很清楚了——增加 ec2 的数量。为了避免不必要的成本，唯一合理的方法是使用自动缩放机制。

回到最初的基础架构——所有组件都是静态的，根本无法扩展。幸运的是，我们的应用程序是 [12 因素](http://web.archive.org/web/20221007194155/https://12factor.net/) 一致的，所以横向扩展几乎是现成的。有了 AWS 自动伸缩机制，根据`CPUUtilization`指标动态生成机器只需几分钟。

下一轮测试把我们带到了下图:

![AWS Aurora RDS](img/0e6b5d61fafb709c0e1d7d7471177383.png)

正如您所看到的，随着计算能力的提高，请求的数量现在看起来好多了，但是我们仍然在与超时做斗争。我们检查了应用程序的日志，看到了许多类似这样的消息:

```
WARNING:  terminating connection because of crash of another server process
DETAIL:  The postmaster has commanded this server process to roll back the current transaction and exit, because another server process exited abnormally and possibly corrupted shared memory.
HINT:  In a moment you should be able to reconnect to the database and repeat your command.
```

这就是问题所在。看起来数据库在高负载下崩溃了，这就是为什么我们仍然在应用程序端收到超时。为了处理这样的流量高峰，我们必须增加数据库吞吐量。通过增加数据库实例的味道，我们将永久增加成本，但是由于我们只需要处理峰值，我们必须找到一些更具可伸缩性的解决方案——这就是 Amazon Aurora 发挥作用的地方。

## 旅程

此时，我们决定采用 Amazon Aurora 解决方案，以经济高效的方式扩展我们的数据层。Amazon Aurora 是一个完全托管的关系数据库引擎，它通过基于嵌入式指标(如`CPUUtilization`或数据库连接)自动添加读取副本来实现数据库可伸缩性。这个简短的视频包含了您完成本文所需的一切:

当然，Aurora 还提供了许多其他很酷的功能，如增量备份或存储自动扩展。然而，这可能是一篇专门针对 Aurora 的独立文章。如果你觉得你更想探索极光，欢迎访问亚马逊官方极光文档[](http://web.archive.org/web/20221007194155/https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)

从 RDS 迁移到 Aurora 后(流程有据可查 [此处](http://web.archive.org/web/20221007194155/https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraPostgreSQL.Migrating.html#AuroraPostgreSQL.Migrating.RDSPostgreSQL.Replica) )我们的基础架构看起来如下:

![AWS Aurora RDS](img/de268be205d6f41903471e96396cfe54.png)

请注意，实际上有两个极光端点。一个用于写，另一个用于读——这是读副本可伸缩性的结果。Aurora 集群中的负载平衡器通过 [循环 DNS](http://web.archive.org/web/20221007194155/https://en.wikipedia.org/wiki/Round-robin_DNS) 在读取副本之间分配读取。理解这一部分至关重要，因为应用程序必须知道这两个端点，以便根据 SQL 查询类型选择合适的 DB 实例。为此，我们在应用程序中使用了[makara](http://web.archive.org/web/20221007194155/https://github.com/instacart/makara)Ruby gem，它允许我们显式地指向读写数据库端点。

每个层级都有如此可扩展的基础设施，应该不会再有瓶颈了，对吧？不对！

在下一组性能测试中出现了以下挑战:

![AWS Aurora RDS](img/49124c84a956445c7d655d84964e8278.png)

正如您所看到的，Aurora 读取副本在大量使用 CPU 的情况下进行了扩展，但是在新的扩展读取副本上没有连接。这是怎么回事？原来，应用程序与之前的读取副本保持 TCP 连接(会话),只有少数连接来自 EC2 自动缩放过程中新启动的应用程序。这个问题可以通过两种方式解决:

1.  更快地扩展读取副本，以便从新启动的 EC2 实例中获得更多连接。
2.  防止创建长期 TCP 会话，并保持它们的短期存在。

第一种解决方案是天真的，等于自找麻烦，因为几乎不可能使 EC2 实例群的扩展与读取副本同步。第二个听起来很好，但是我们必须找到一种机制，在特定的时间后拆除长期存在的 TCP 会话。具体到什么程度取决于您的系统和需求。例如，允许连接存活时间不超过 10 分钟意味着在第二个读取副本联机后大约 10 分钟，负载应该恢复平衡。

在深入研究了[makara](http://web.archive.org/web/20221007194155/https://github.com/instacart/makara)gem 和[ActiveRecord](http://web.archive.org/web/20221007194155/https://guides.rubyonrails.org/active_record_basics.html)中的许多设置之后，我们最终发现了 active record 中的两个重要参数:

*   `idle_timeout` - 在自动断开连接之前，连接在池中保持未使用状态的秒数(默认为 300 秒)。
*   `reaping_frequency` - 定期运行 [收割者](http://web.archive.org/web/20221007194155/https://api.rubyonrails.org/v5.1.7/classes/ActiveRecord/ConnectionAdapters/ConnectionPool/Reaper.html) 的频率(秒)，它试图从死线程中找到并恢复连接，如果程序员忘记在线程结束时关闭连接或线程意外死亡，就会发生这种情况。

> 这是特定于 Ruby on Rails 应用程序的，然而 [在这里](http://web.archive.org/web/20221007194155/http://cloud.google.com/sql/docs/postgres/manage-connections#duration-python) 你可以找到其他编程语言有用的数据库连接参数。

我们将每个设置为 5 秒，并再次进行性能测试。

![AWS Aurora RDS](img/b8942790dcf1e10bd57865aa733f4234.png)

瞧啊！最后，流量在读取副本之间分布良好！

灾难恢复

## 这样的设置足以用自动缩放机制处理请求的流量，但是我们希望确保我们的系统能够处理所有这些用户，即使当它的一部分中断时。应用程序层已经包含了灾难恢复计划，因为应用程序的每个损坏实例都可以在自动扩展组中轻松自动地替换。更有趣的部分是数据层。为了模拟一个数据库实例的崩溃，我们使用了 Amazon 的嵌入式故障转移机制。不幸的是，在故障转移之后，即使启用了`reaping_frequency`设置，应用程序仍然保持 TCP 会话。这导致大量应用程序实例具有大量未使用的打开的 TCP 会话，并填满了连接池，从而导致客户端超时。

*旁注:*为了保持这个故事的合理简短，请参考下面的故事，在这些故事中，其他人也遇到了类似的问题:

> 这是不可接受的，所以经过一些调试后，我们决定放弃 makara gem，将连接池与第三方中间件软件分离。

连接池

## 在我们进入中间件软件部分之前，我们必须了解连接池实际上是什么，以及它为什么如此重要。

你可以从 [这篇伟大的文章](http://web.archive.org/web/20221007194155/https://sudhir.io/understanding-connections-pools/) 中获得关于连接池的杰出知识，但是为了使它简短，我将在这里摘录一段引文，只是粘贴一段引文，以便让你理解我们正在谈论的内容:

池是在内部维护一组连接的对象，不允许直接访问或使用。当需要与数据库进行通信时，这些连接由池发出，并在通信结束时返回到池中。该池可以用配置的连接数进行初始化，也可以按需缓慢填充。连接池的理想用法是，代码在需要使用时从池中请求连接(称为签出)，使用它，然后立即将它放回池中(释放)。这样，当所有其他与连接无关的工作都在进行时，代码就不会占用连接，从而大大提高了效率。这允许使用一个或几个连接来完成许多工作。如果在请求新的签出时池中的所有连接都在使用中，通常会让请求者等待(将阻塞)直到一个连接被释放。

> PostgreSQL 中间件

## 由于我们的系统变得越来越复杂，而且应用程序不能处理数据库故障转移，我们决定将连接池与第三方软件分离。PostgreSQL 有两个流行的代理:

这些系统允许你建立尽可能多的数据库连接，而不用担心管理问题，因为它们给你的连接是廉价的模拟连接，处理开销很低。当您试图使用这些模拟连接之一时，它们会从内部池中拉出一个真实的连接，并将您的假连接映射到一个真实的连接上。在代理看到您已经使用完连接之后，它会保持您的假连接打开，但是会主动释放并重用真正的连接。连接数和释放积极性设置是可配置的，有助于您针对事务、预准备语句和锁等问题进行优化。

对于可伸缩的亚马逊 Aurora 数据库，只有 PgPool 适合，因为 PgBouncer 不支持读/写 SQL 查询拆分。PgPool 的配置一开始可以让人应接不暇，但是有一套专门为极光 设置的 [可以作为一个很好的起点。特别值得关注的是`num_init_children`和`max_pool`参数，它们表明可以打开多少个数据库连接。关于参数之间的关系以及如何根据您的需要设置它们的更多信息可以在](http://web.archive.org/web/20221007194155/https://www.pgpool.net/docs/latest/en/html/example-aurora.html) [这里](http://web.archive.org/web/20221007194155/https://www.pgpool.net/mediawiki/index.php/Relationship_between_max_pool,_num_init_children,_and_max_connections) 找到。

经过一些配置调整和一些性能测试，我们最终实现了目标:

After some configuration tuning and a couple more performance tests we finally managed to achieve the goal:

![AWS Aurora RDS](img/b88405acdf96a2eaa629f72571345810.png)

如您所见，连接均匀分布到副本，在故障切换会话期间，故障实例的连接被释放并立即切换到其他副本。

结论

## 几经波折后，我们实现了防弹的可扩展基础设施，在数据库和应用程序级别上具有弹性。该系统能够以经济高效的方式处理超过 25k 的并发用户，因为它可以根据系统负载自动扩展。本旅程描述旨在提供一个真实的例子，说明如何设计和改进基础设施系统，以及中间的所有起伏。

这个故事还没有结束，因为我们还有很多可以调整的地方，比如复制延迟。所以我想给你留下一句话，我认为这篇文章说明了:

重要的不是如何结束，而是到达目的地的旅程。

> 照片由 [泰勒维克](http://web.archive.org/web/20221007194155/https://unsplash.com/@tvick) 上 [下](http://web.archive.org/web/20221007194155/https://unsplash.com/)

Photo by [Taylor Vick](http://web.archive.org/web/20221007194155/https://unsplash.com/@tvick) on [Unsplash](http://web.archive.org/web/20221007194155/https://unsplash.com/)