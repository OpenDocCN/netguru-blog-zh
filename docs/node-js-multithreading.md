# Node.js 走向多线程:JavaSript 后端框架的未来

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/node-js-multithreading>

 有没有考虑过使用 Node.js 进行 CPU 密集型计算？如果是这样，你必须意识到这不是最好的方法。然而，随着 Node.js 的新面孔出现在地平线上，这种情况可能会在未来发生变化。

Node.js，一个单线程的 JavaScript 运行时，提供了许多优势。它具有出色的非阻塞 I/O 性能。它也是轻量级和高效的，它为实时解决方案提供了无与伦比的支持，如 sockets.io 或 WebRTC。它带有最大的包装生态系统。非常适合开发微服务架构。所有这些特性使 Node.js 成为一项伟大的技术，但是 Node.js 在一个领域失败了。单线程非常适合处理异步代码，但对于 CPU 密集型计算来说肯定不是最佳选择。在 Node.js 中，任何耗时的计算都会阻塞事件循环，从而阻塞整个应用程序。

来自 [Node.js 社区](/web/20220927120223/https://www.netguru.com/blog/how-the-node.js-developer-community-can-help-your-app-succeed)的最新消息让我们认为这在不久的将来将会改变。10.5.0 版本宣布了 Node.js 中的多线程功能。该功能仍处于试验阶段，可能会经历广泛的变化，但它确实显示了 Node.js 的发展方向。

最新版本引入了 worker 模块，它公开了代表独立 JavaScript 执行线程的 worker。Workers 使得在多线程上运行代码以及通过消息通道同步这些代码成为可能。workers 旨在处理 CPU 密集型操作，并不意味着处理异步 I/O。此外，与集群模式或进程不同，workers 可以高效地共享内存。如果您希望深入了解，请关注 [官方文档](http://web.archive.org/web/20220927120223/https://nodejs.org/api/worker_threads.html) 。

Node.js 吹嘘自己是单线程的，为什么现在要实现一个与其基本假设完全相反的特性呢？我的想法是，一旦多线程完全发挥作用， [Node.js 应用程序](/web/20220927120223/https://www.netguru.com/blog/node-js-apps)将从单线程和多线程中受益。换句话说，Node.js 将保持单线程，除非它决定以其他方式来适应特定的业务需求。最后，Node.js 将为图形、复杂数据处理和[机器学习](/web/20220927120223/https://www.netguru.com/blog/when-to-use-machine-learning)腾出空间。