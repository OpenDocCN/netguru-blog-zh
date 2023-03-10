# 异步编程

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/introduction-asynchronous-programming>

 介绍

本文将严格讨论编程中的异步概念。在本文中，不会有任何关于如何处理异步代码的特定语言解决方案。我将在接下来的一系列关于 JS 异步代码的简明笔记中以 JavaScript 为例介绍这些内容。别再耽搁了，让我们进入正题吧。

为了更好地理解异步性，让我们讨论一些关于计算机如何实际排序连续代码块执行的基础知识。负责处理我们开发人员编写的代码的主要单元是处理器。在讨论代码中所有已知变量的简单操作时，程序执行的时间直接取决于处理器的能力。但是有些程序使用其他来源的数据，例如通过网络通信或从硬盘驱动器请求数据。其中一些操作可能需要很长时间，处理器在等待时会处于空闲状态——这绝对是浪费时间。

还有一个重要的概念需要添加——单线程和多线程环境(不要与同步和异步**编程模型**相混淆)。例如，同步(或异步)模型可以用在单线程或多线程系统中。

## **同步执行**

同步动作意味着它们是同步的，例如与一个主时钟同步。这意味着访问代码关键部分的线程或进程必须与其他线程或进程同步，也就是说，要确保在任何给定的时间，只有其中一个线程或进程可以执行该部分代码。线程是独立运行的程序(在进程中具有独立值的最小代码指令序列)，它可以通过操作系统与其他程序交错。

在这个编程模型中，在给定时刻(在每个线程上)只能发生一件事。上图展示了单线程同步方法——一个线程只有在完成前一个任务后才会处理下一个任务。基本上，程序是一系列连续执行的代码指令。

函数执行可能需要很长时间。这意味着，代码的其余部分将一直等待，直到它的执行完成——例如，可能会阻塞代码流中不依赖于该函数成功完成的部分。这当然是一个问题，但不是唯一的问题。如果函数执行不完怎么办？程序可能会卡住。好消息是，如果我们使用正确的方法来解决问题，就不会这样。

在同步系统中，我们可以提出启动额外的控制线程的解决方案。如果我们开始添加更多的线程(可用线程的数量取决于机器的规格、CPU 和内存)，那么这些任务就可以像下面的图片一样并行执行。

在这种环境下，每个线程可以选择单独的任务并处理它，直到完成，然后，当再次可用时，可以执行下一个任务。这也意味着，例如，我们可以有两个函数，它们将执行一些操作，然后返回它们的结果，重新同步并在将来的执行中使用它们。这可能是始终优先选择的解决方案，但是，如前所述，运行额外的线程将在某种程度上取决于机器规格，同时对于开发人员来说，保持正确的同步和防止崩溃(死锁、活锁、资源争用)也更加困难。

## **异步执行**

在异步编程模型中，事情也可能看似同时发生。异步操作在继续之前不会等待其他任务完成——一个线程实际上不会停止它的工作——它会切换到其他可用的任务。不过，在单线程环境中，这不会像在多线程环境中那样是真正的并行。当主代码执行一些辅助动作时，通常会等待结果——它继续在这里运行。因此，实际上异步指令与代码的其余部分同时运行(虽然是分开的)。这可能发生在一个线程上，该线程可以在任务之间切换，并获取执行进一步代码指令所需的结果。有一个很好的类比来解释这种差异。想象一下，你是一个厨师(线程)，有一个准备鸡蛋和烤面包的订单(程序)(任务)。

同步方式您:

1.  煮鸡蛋，等待它们。
2.  做好烤面包，等着他们。
3.  上菜。

异步单线程您:

1.  用定时器设定煮蛋。
2.  用定时器设定烤面包片烹饪。
3.  你可以在等待的时候打扫厨房(或者做其他任何事情)。
4.  把鸡蛋和烤面包从火上拿开(就像收集数据一样)
5.  上菜。

接下来是更技术性的例子，网络或硬盘驱动器对数据的请求可以在程序仍在运行时使用异步调用来进行。看上面的图片，假设任务 1 (T1)需要一些来自网络的信息，它调用另一个函数来做这件事，这个函数可以用 T2 来表示。线程保存 T1 的状态并执行 T2，然后将其结果返回给 T1，程序继续处理获得的数据。所以实际上，每当需要执行异步动作的函数被调用时，它会返回一些值，或者它可以根据我们传递给它的指令执行一个额外的动作——这些是回调函数。此外，如果代码在多线程环境中执行，那么所有线程都可以利用异步编程模型在它们之间传递数据。

![async-single](img/040fb91dd0dc90204a41f1f88605a02e.png)

现在，当我们知道什么是异步编程时，让我们快速看一下它能给我们带来的好处。对于任何应用程序来说，最重要的当然是可用性——也就是说应用程序对用户来说有多友好，用户是否觉得它使用起来流畅和愉快。这方面的一个很好的例子可能是单击按钮，导致运行将执行外部资源请求的代码。在这种情况下，同步编程将无法使用，因为在数据返回之前，用户界面将被阻止进一步使用。异步方法允许用户在获取数据时在 UI 中自由移动，这是为什么现在这么多应用程序和框架使用这种模型的主要原因。

另一个重要的优势是应用程序整体性能的提升。当应用程序执行请求时，注意到大约 70-80%的工作时间会浪费在等待外部任务上。因此，这一时间可以通过异步方法来节省——当任务将进一步的执行委托给其他进程时，它会保存其状态，并可以处理下一条指令。然后，任何可用的线程都可以继续执行返回的任务(在多线程环境中)。

另外一个值得注意的好处是，不在直线上运行的程序可以用一种更方便、更易读的方式来表达。然而，有许多情况不需要使用异步方法，甚至会使事情变得更加复杂。

**何时使用/不使用异步编程**

异步编程是一件伟大的事情，但是就像科技领域的任何事情一样，有利也有弊。最近，这种模式被许多程序员越来越频繁地使用。虽然它有助于解决许多用例，但也可能导致一些意想不到的问题，因此它不应该在所有可能的场景中作为一个现成的解决方案使用——特别是在您不确定是否需要它的情况下。

## 话虽如此，我还是会在下面重点介绍几个*该做和不该做的*点:

异步更新依赖或被依赖的记录通常不是一个好主意，因为它迟早会导致您的数据不同步和混乱。最好用它来处理独立的数据。

对于预加载数据非常有帮助，同时向用户显示应用程序的其他内容，但在移动设备上要小心，不要用异步操作使应用程序过载，因为这些操作更消耗资源。

1.  如果您使用没有连接池的单一数据库，那么使用异步调用就没有任何意义，因为服务器仍然是瓶颈。
2.  如果应用程序将使用简单、短时间运行的操作——特别是仅由 CPU 执行的操作——你不应该选择异步。
3.  除了上面的一个——如果你的应用程序可以在获取数据时停止(不适合 UI 应用程序)——你应该考虑不使用异步方法。
4.  当任务数量很大时，特别是当它们可能互相阻塞时，使用异步。
5.  现在，随着我们对异步编程和线程有了一些了解，我邀请您阅读我以后的帖子，继续 JavaScript 示例的主题——敬请关注！

照片由 [萨布里](http://web.archive.org/web/20221004130249/https://unsplash.com/@sabrituzcu?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](http://web.archive.org/web/20221004130249/https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Photo by [Sabri Tuzcu](http://web.archive.org/web/20221004130249/https://unsplash.com/@sabrituzcu?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](http://web.archive.org/web/20221004130249/https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)