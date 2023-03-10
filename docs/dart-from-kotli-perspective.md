# 科特林视角下的飞镖刺激

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/dart-from-kotli-perspective>

 几年前我第一次用 Flutter 看 Dart 的时候，我必须说我对此非常怀疑。像科特林这样的新语言中有很多我喜欢的东西是达特所没有的。Egipt 中的嵌套窗口小部件让代码看起来像金字塔一样，但现在我认为它比以往任何时候都更有可能成为我的最爱之一。我来解释一下原因。

![nathan-dumlao-1112682-unsplash](img/d5b05719d24623a804b7eee0efc747d6.png)

## 串联

这两个点符"..“可以省去你创建一个构建器或者只是编写更可读的代码。
它允许你对同一个对象进行一系列的操作。类似的东西我们可以用它来实现

申请{...}kot Lin 中的扩展函数。

**中的“T1”**

```
val person = Person().apply { 
    name = "Marcin" 
    surname = "Oziemski"
} 
```

**飞镖:**

```
final person = Person() 
    ..name = "Marcin" 
    ..surname = "Oziemski" 
    ..age = 99
```

## 
传播

我在 JavaScript 中第一次遇到的 **spread** 操作符扩展到了 Kotlin，现在(从 2.3 版)扩展到了 Dart。

Kotlin 中的符号不同，用星号(*)表示，但背后的思想是相同的。它打开一个项目集合。

在 **Kotlin** 据我所知，只有数组才有可能:

```
val s = arrayOf("d", "e", "f")
val s2 = listOf("a", "b", "c", *s)
println(s2) // [a, b, c, d, e, f]
```

在普通扩展操作符旁边的 **Dart** 中，我们也有一个空感知版本...？此外，它还可以操作更多种类的集合，如集合或地图。

```
final s = ["d", "e", "f"];
final s2 = ["a", "b", "c", ...s];
print(s2); // [a, b, c, d, e, f]
```

```
var s;
final s2 = ["a", "b", "c", ...?s];
print(s2); // [a, b, c]
```

不过，它是一种静态类型的语言，不能像 JS 那样神奇地打开对象。

## 可空性

Kotlin 的类型系统旨在消除代码中空引用的危险。

我们还没有 Dart 中的那个。在 Google I/O 2019 上，他们宣布他们正在研究不可空类型！！！多棒啊。！👏😲

除了类型本身，它们还有类似于 Kotlin 中的基本空检查操作符:

1.  猫王在科特林的操作员？:那个双问号是吗？？中镖。
2.  有条件的会员准入？。在他们俩身上是一样的。

Kotlin 有一些更像非空断言操作符！！还是安全将投**为**？，但是 Dart 有“if null 赋值”？？= 仅当赋值给变量为空时赋值。

```
// Assign a value to b if b is null; otherwise, b stays the same
b ??= value;
```

## 米欣

Mixin 在 Dart 中并不是一个新东西，但是对于我所知道的大多数语言来说，它是一个新概念。我能想到的只有一种类似的东西可以在 Kotlin 中通过委托来实现，即通过构造函数 param 来实现接口**。**

```
class Bird(wings: Flying) : Animal, Flying by wings
```

如果你想更多地了解科特林的代表们，这篇文章很棒。

好的，但是让我们回到什么是混合？我们都知道一个类只能扩展一个类，但是如果我们想扩展额外的带有状态和函数的类型到我们的类中呢？这就是 mixin 的用武之地！

就像上面 Kotlin 例子一样，我们可以扩展父类，但是不是通过实现接口的参数添加额外的状态和功能，而是添加 mixin。

```
class Bird extends Animal with Wings
```

那么 Wings mixin 可能是这样的:

```
mixin Wings { 
    String speed = "fast" 

    void fly() { 
        print('Flying $speed') 
    }
}
```

如果你需要更多关于什么是混合的解释，请查看这篇文章。

## 针对用户界面优化

这是来自新 [dart.dev](http://web.archive.org/web/20221007153610/http://dart.dev/) 网站的口号，在新的 Dart 2.3 中，这比以前更加真实。
在这篇文章的开始，我写道我对构建窗口小部件的第一印象是看起来像金字塔的代码，但是有了这个新版本，我们可以用许多方式消除嵌套窗口小部件。

我们可以在小部件声明中使用 if，for 和 spread 运算符:

```
Widget build(BuildContext context) { 
    return Column(children: [ 
        Text(mainText), 
        ...buildMainElements(), 
        if (page != pages.last) 
            FlatButton(child: Text('Next')), 
        for (var section in sections) 
            HeadingAction(section.heading), 
    ]);
}
```

欲了解更多详细信息，请访问这篇文章。

## 摘要

除了我刚刚描述的几件事情之外，还有 **async-await** 、 **RxDart** 和许多其他事情，这些事情让我对 Dart 及其进展方式越来越感兴趣。
[我觉得随着 Flutter 的人气越来越高，Google 和开源社区](/web/20221007153610/https://www.netguru.com/services/flutter-app-development)(正如 Dart 是[开源](http://web.archive.org/web/20221007153610/https://github.com/dart-lang))真的可以让它在人气上一争高下。