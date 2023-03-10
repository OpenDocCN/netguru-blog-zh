# 如何在 JRuby Gem 中包装任意 Java 类？

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-to-wrap-arbitrary-java-class-in-jruby-gem>

 最近，我偶然发现了一个 Java 类，它正在执行我在开始编写我的 gem 时想到的任务。该课程从 PDF 中提取文本，同时保留文本结构。我曾经是一名 Java 开发人员，但是我希望我的项目仍然使用 Ruby。

"Let's wrap it in JRuby gem!" - came to my mind. I started googling and found excellent tutorials on this topic. However, each of them covered wrapping jar package, rather than single class. I started looking for the solution even deeper and found answers in different places on the web. I decided to wrap it in this post.

So firstly, let me introduce The Java Class: [PDFLayoutTextStripper](http://web.archive.org/web/20221201140524/https://github.com/JonathanLink/PDFLayoutTextStripper). This class is very standard (when it comes to Java world standards). One important thing that it's missing is package definition. Packages in java world can be translated to modules in Ruby. The tutorial I found, assumed every Java class is namespaced by package name - and to be honest I didn't want to change the class signature. I spotted a challenge here :)

好，我们开始吧。我提到了宝石，对吧？但是在我们创建一个 gem 之前，我们需要确保我们使用的是 JRuby:

`❯ ruby -v
jruby 9.1.12.0 (2.3.3) 2017-06-15 33c6439 Java HotSpot(TM) 64-Bit Server VM 9.0.4+11 on 9.0.4+11 +jit [darwin-x86_64]`

为了创建一个 gem，我采用了 Bundler guide 中提到的标准方法:
`❯ bundle gem pdf-textstream # naming things is a second hardest thing in IT, right?`
遗憾的是，因为我们将使用 Java 本地代码，所以我们的 gem 将只兼容 JRuby。为了确保它只在 JVM 上执行，您必须修改 pdf-textstream.gemspec 文件并设置平台参数:
spec . platform = ' Java '

包装器代码将驻留在 lib/pdf/textream.rb 中。让我一行一行地告诉你。
`require "pdf/textstream/version"
require "java"`

要使用 Java 类(还有 Java stdlib，甚至直接引用 Java 代码)，我们就必须要有 Java 模块。

接下来的事情就是以 ruby 的方式要求 Java jar:
`# load jars
require_relative "../../jars/pdfbox-2.0.6.jar"
require_relative "../../jars/commons-logging-1.2.jar"
require_relative "../../jars/fontbox-2.0.6.jar"`
那些是被引入类的依赖。当然，你必须下载它们并把它们放在‘jars’目录中，然后把它们的编译版本和你的 gem 一起发布。

接下来重要的一行是类路径定义:
`$CLASSPATH << "#{File.expand_path(File.dirname(__FILE__))}/../../classes"
module Pdf
module Textstream` 
类路径，对于有 Java 背景的人来说，相当简单明了。它是目录，JVM 在其中寻找包含的库。事实上，在我们的项目中没有名为 classes 的目录。Java 编译器会自动创建它。但是我们仍然没有编译器。
可能——这不是最佳实践，但是我包含了执行以下命令的构建文件:

`javac -d classes -cp .:./jars/pdfbox-2.0.6.jar:./jars/commons-logging-1.2.jar:./jars/fontbox-2.0.6.jar *.java`

每次修改 Java 类或更改依赖项时，您都应该手动执行该命令。最后，神奇的比特。首先，将 Java 类复制到您的 gem 的根目录。然后，通过使用 JRuby 作为代理，我们可以引用它:
`PDFLayoutTextStripper = JavaUtilities.get_proxy_class("PDFLayoutTextStripper")`

我做的下一件事，是我使用的类的缩短名称空间。每个 Java 类都可以通过 Java 模块树以 Ruby 的方式引用:
`# change namespace
PDFParser = Java::OrgApachePdfboxPdfparser::PDFParser
RandomAccessFile = Java::OrgApachePdfboxIo::RandomAccessFile
PDDocument = Java::OrgApachePdfboxPdmodel::PDDocument
PDFTextStripper = Java::OrgApachePdfboxText::PDFTextStripper`
为了执行类，并在位于给定路径的文件上运行它，我创建了一个静态方法:

```
def self.file_path_to_text(path)
    # TODO: exception handling
    pdfParser = PDFParser.new(RandomAccessFile.new(Java::JavaIo::File.new(path), "r"))
    pdfParser.parse()
    pdDocument = PDDocument.new(pdfParser.getDocument());
    pdfTextStripper = PDFLayoutTextStripper.new
    string = pdfTextStripper.getText(pdDocument);
    return string
end 
```

它启动 PDF 阅读器，解析 PDF 文件，将文档传递给我们的任意类，并返回它读取的字符串。最棘手的部分是我试图将 Ruby 文件句柄作为参数传递给 PDFParser。当然失败了。PDFParser 签名需要来自 Java 世界的文件句柄。这对我来说是新的东西，这就是为什么我必须阅读文件“Java 方式”:randomaccessfile . new(Java::JavaIo::file . new(path)，" r")

还有…就是这样！您的 Java 类打包成宝石，随时可以使用！你可以在我的 [GitHub 回购](http://web.archive.org/web/20221201140524/https://github.com/mic-kul/pdf-textstream)里找到宝石。请记住，它是作为概念验证而创建的，并不准备用于生产。

* * *

照片由[Max Nelson](http://web.archive.org/web/20221201140524/https://unsplash.com/photos/taiuG8CPKAQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上 [Unsplash](http://web.archive.org/web/20221201140524/https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)