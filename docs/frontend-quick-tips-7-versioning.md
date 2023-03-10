# 前端快速提示#7 版本控制——如何让您的项目经理在部署后微笑

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/frontend-quick-tips-7-versioning>

 没有人喜欢那些大文章——这就是为什么我们创建快速提示——从你阅读它们的那一刻起改变你的开发者生活的简短提示。这些可能是 JS 代码中解释的一些模式，或者是一些更好的代码的技术。

 问题

## 发布新版本的应用程序时，可能会问你一个问题:

客户:“好的，那么自从上次发布以来你实际上做了什么？”

你:“嗯……很多！”

你可能做了很多改变/特性，但是你想要更多的信息。

另一个问题包括无法跟踪应用程序的哪些实例提供了哪些功能。你的最新作品包括所有的新功能吗？这个 bug 修复已经发布到测试环境了吗？嗯，现在我必须挑选所有这些提交…

解决办法

## 要解决这些(以及更多)问题，您可以做的事情很少:

在每个版本上更新你的应用程序——你可以很容易地从一个部署的版本跳到另一个，

*   增加提交的可读性，
*   创建一个 changelog 来跟踪自上次发布以来的更改。
*   Create a changelog to track changes since last release.

![conventional commits](img/5a516100ebd7b1296988dc69ed4f1b88.png)You can use the [https://www.conventionalcommits.org/en/v1.0.0/](http://web.archive.org/web/20220925011306/https://www.conventionalcommits.org/en/v1.0.0/) convention to make sure your commits are readable and unified.![commits](img/7fae5c379628c2ed0de3396b8ec6ee99.png)← Here is an example of the syntax

您可以使用预定义的前缀之一:

更多(查看文档)。

```
feat, fix, style, chore 
```

然后是门票的范围(可选)和说明(我也建议使用吉拉门票参考)。

所以模式应该是:

那很好，但是不够酷！

```
prefix(scope): description.

```

## …这就是为什么你可以把它和这个很酷的插件 [传统版本/标准版本](http://web.archive.org/web/20220925011306/https://github.com/conventional-changelog/standard-version) 结合起来

在一条命令上(我喜欢使用 package.json 脚本

)你可以升级新版本的应用程序，并根据你在 Markdown 中的提交自动创建一个 changelog。没错。不仅如此——插件会自动计算如何修改你的应用程序版本(如果只有修正，它只会修改)

```
"release”: ”standard-version”
```

如果有功能的话，它会把版本提高 20%

```
x.x.1; 
```

附加说明:

```
x.1).
```

你可能听说过我们 ReactArea 团队关于新项目的 CRA 模板的最新工作。它从一开始就包含了这个插件。

![features, bug fixes](img/6c3b673a3ce8487c508a145ccd0c278d.png)Here is an example of my project’s latest changelog (all styles, chores and tests are hidden - fixes and features are grouped). And since it creates a CHANGELOG.md file in your repo, you can just simply send a link from Github to your PM. Trust me - they will love it.

附注 2

### 还有一个类似的包[emantic-release/semantic-release](http://web.archive.org/web/20220925011306/https://github.com/semantic-release/semantic-release)做了几乎相同的事情，尽管它也自动发布/发布你对遥控器的更改。有了标准版，你可以更好地控制你所推送的内容，但是你可以自由地尝试找到最适合你的解决方案。

更多[此处](http://web.archive.org/web/20220925011306/https://www.npmjs.com/package/standard-version#how-is-standard-version-different-from-semantic-release-)。

There is also a similar package [emantic-release/semantic-release](http://web.archive.org/web/20220925011306/https://github.com/semantic-release/semantic-release) which does mostly the same thing, although it also automatically releases/publishes your changes to the remote. With standard-version you can have more control over what you push, but feel free to experiment to find the solution that suits you the best.

More [here](http://web.archive.org/web/20220925011306/https://www.npmjs.com/package/standard-version#how-is-standard-version-different-from-semantic-release-).