# 前端快速提示#5 管道/组成-正确的方式来转换你的价值观

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/frontend-tips-5-pipe-compose-transform-values>

 没有人喜欢那些大文章——这就是为什么我们创建快速提示——从你阅读它们的那一刻起改变你的开发者生活的简短提示。这些可能是 JS 代码中解释的一些模式，或者是一些更好的代码的技术。

Problem?

我们必须转换我们的值，并将每个函数的结果作为下一个函数的参数传递。

![Q5](img/689d4213d5660b766c472cabc3662921.png)

这是不可伸缩的，需要一些样板代码来正确处理。

## 解决方案？

将函数的结果传递给下一个函数的管道模式(类似于用 then/catch 链接承诺时使用的模式)。

![q51](img/e645dfd8f2b8855323781349587a298b.png)

更加优雅，可读性更强。如何创建这样的管道函数？这里有一个你可以使用的片段:

`export const pipe = (...fns) => args => fns.reduce((result, nextFun) => nextFun(result), args);`

花点时间去理解这个很小但相当混乱的函数。

首先，我们获取一个函数列表，并返回一个运行所有函数的函数。通过使用 reduce，我们可以将前一个函数的结果传递给下一个函数。

### *作曲*呢？

还有一个替代方法——`*compose*`方法，它有相似的主体和结果

`export const compose = (...fns) => args => fns.reduceRight((result, nextFun) => nextFun(result), args);`

唯一的区别是数据流的方向。通过管道，数据从第一个函数流向另一个函数。使用 compose，它从最后一个开始，流向第一个。你为什么要逆转水流呢？为了可读性——这在 React 中很常见。

![q52](img/d1d803941af2613f720fea6afd533908.png)

我们正在读取包装我们组件的内容。

下面两张图展示了与的区别:

[https://medium . com/@ AC paras/what-I-learned-today-July-2-2017-ab 9a 46 DBF 85 f](http://web.archive.org/web/20221007191619/https://medium.com/@acparas/what-i-learned-today-july-2-2017-ab9a46dbf85f)

![pipe compose](img/86a429ef38d0fed04f561fc0644995f8.png)![q54](img/97d29e56462290c864db0acd7770f4ac.png)

## 注意

这种模式在 Rxjs (observables)中被广泛使用，然而在 web 上的一些例子中可以找到几乎相同的管道实现。这个代码片段的实现不是我的功劳。

更详细的解释:[https://vanslaars.io/blog/create-pipe-function/](http://web.archive.org/web/20221007191619/https://vanslaars.io/blog/create-pipe-function/)