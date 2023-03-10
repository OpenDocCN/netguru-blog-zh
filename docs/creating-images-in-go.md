# 在 Go 中创建图像

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/creating-images-in-go>

 本帖是最初出现在[上的围棋](http://web.archive.org/web/20221202083142/https://szeliga.github.io/2016/09/02/creating-images/)中的[向量运算经过八次编码后的延续。](http://web.archive.org/web/20221202083142/https://www.netguru.com/blog/vector-operations-in-go)

## 序

在实现相机模型之前，最好有一些调试工具。当编写光线跟踪器时，该工具是一个渲染图像。

Go 有一个内置的[图像](http://web.archive.org/web/20221202083142/https://golang.org/pkg/image/)包，允许轻松创建图像并将其保存为磁盘上的文件。

这篇文章的代码可以在这里查看[。](http://web.archive.org/web/20221202083142/https://github.com/Szeliga/goray/tree/03-creating-images)

## 设计

该场景将由一个结构来表示。在内部，它将存储所需图像的宽度和高度，以及一个指向图像实例的指针。RGBA 。

[代码]

### 初始化中

为了初始化场景，我们需要初始化图像。给定尺寸的 RGBA 。在 Go 中，这是通过创建一个图像来实现的。Rect struct 并将其传递给映像。NewRGBA 功能。

初始化场景所需的全部代码将在 NewScene 函数中完成。该函数应该接受两个整数作为参数，表示图像的宽度和高度:

[代码]

assert 库公开了一些漂亮的帮助函数，如[objects areequalvalues](http://web.archive.org/web/20221202083142/https://godoc.org/github.com/stretchr/testify/assert#ObjectsAreEqualValues)，对对象的值进行深度相等检查。

### 测试助手

为了测试下面的函数，我需要编写两个助手函数来生成一个新的图像。RGBA 并用随机颜色填充图像:

[代码]

我使用这些助手来生成预期的图像。RGBA 的对象为的对象为的对象。

### 遍历图像

对于图像的每个像素，我想设置一个特定的颜色。这可以通过一个双 for 循环简单地完成，但是我们可以在场景上公开一个方便的函数，叫做每个像素。Go 允许我们将函数指定为其他函数的参数。我们将利用这一事实，在每个像素中需要一个参数——一个具有以下签名的函数 func(int，int) color。RGBA 。两个 int 值是图像的 x 和 y 坐标(稍后将用于计算图像平面中的像素)。 EachPixel 函数的完整实现如下:

[代码]

setPixel 函数只是 Go 内置图像的一个适配器。RGBA .设定。

### 保存图像

最后，我想将图像保存到一个 PNG 文件中。正如人们可能怀疑的那样，Go 在 image/png 包中提供了这样一个函数，叫做 png。编码。同样，我为这个函数构建了一个适配器，它接受一个文件名，在这个文件名下保存图像。

[代码]

这段代码中发生了几件有趣的事情:

*   os。Create(filename) —函数有多次返回；这是 Go 中[错误处理的常见模式:](http://web.archive.org/web/20221202083142/https://blog.golang.org/error-handling-and-go)
    *   f —文件描述符，
    *   err —文件创建过程中可能发生的任何错误，例如权限不足；
*   defer f . Close()—[defer 关键字](http://web.archive.org/web/20221202083142/https://tour.golang.org/flowcontrol/12)延迟执行传递的代码，直到周围的函数返回；在这种情况下，文件不会关闭，直到 Save 函数执行完它的命令；
*   断言。panics—assert 库的另一个很好的特性，不是获取断言的预期/实际值，而是获取一个应该会死机(引发异常)的函数，如果这个函数确实死机了，它就通过了测试；这类似于 RSpec 的 expect { subject }。提高 _error 标准错误；

### 把所有的放在一起

下面是一个简单的 main 函数，它使用基于当前像素的值填充图像:

[代码]

这段代码生成了以下图像:

![image.png](img/8eba4ebf2f185c54bbbc31cd4e8772d6.png)

以上是关于在 Go 中创建和保存图像的全部内容。接下来，一个基本的相机模型，敬请关注。