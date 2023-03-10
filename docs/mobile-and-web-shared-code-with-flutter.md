# 使用 BLoC 将 Flutter 移动应用程序转换为 Web

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/mobile-and-web-shared-code-with-flutter>

 Flutter BLoC 是一个很棒的建筑模式，受到了社区的热烈欢迎。但它不仅仅是为移动应用程序而创建的。它是作为一种模式引入的，允许在 Flutter 和 AngularDart 应用程序之间共享多达 50%的代码。在这篇博文中，我将分享[将我的 Flutter 转换到 web 应用程序的经验，并将这个解决方案与 Flutter For Web](/web/20221209114049/https://www.netguru.com/services/flutter-app-development) 进行比较。

与手机和网络共享代码

## 在这篇博文中，我将使用我在 BLoC 教程中创建的应用程序。如果你不熟悉 BLoC 模式，或者如果你想更好地了解我将要转换的应用程序，我建议你先看一看。完成的共享代码应用程序可以在这个[存储库](http://web.archive.org/web/20221209114049/https://github.com/kkogut95/bloc_lyrics_shared)中找到。

只有当你想在手机和网络上有相同的行为时，共享的应用程序代码才能被应用。当然，您可以为一个平台创建不同状态、事件和阻塞，但是创建太多的状态、事件和阻塞会降低共享代码的利润。您的应用程序也必须基于 BLoC 模式构建。如果它是基于 [BLoC 库](http://web.archive.org/web/20221209114049/https://bloclibrary.dev/#/)创建的就更好了，该库在 Flutter 和 AngularDart 上都受支持。

由于移动和 web 应用程序将以相同的方式运行，并具有相同的状态和事件，因此它们可以共享除 UI 类之外的所有内容。请记住，标准共享块应用程序的图表可能如下所示:

![Untitled Diagram (3)](img/78318e32f5a1dbc218f91c3308f952c8.png)

正如您在上面的图表中看到的，提供数据和应用程序状态的所有三个层都在移动和 web UI 之间共享。

将 Flutter 应用程序转换为共享代码

## 开始使用共享代码时，我必须做的第一件事是将所有的共享类移动到一个新的文件夹中，这个文件夹应该创建在移动和 web 应用程序所在的位置。所以最后，包含应用程序的文件夹应该是这样的:

The first thing I had to do to start working with shared code is to move all of the shared classes to a new folder, which should be created in the same place that mobile and web applications will be. So in the end, a folder containing the application should look like this:

![](img/c830230d688c38cb5e22bb9e329530f9.png)

可以在 web 和 mobile 之间共享的类可以是提供数据和管理应用程序状态的所有类。因此，所有的块、模型、存储库和数据提供者都可以移动到 **common_bloc_lyrics** 。

之后，我必须添加一个 pubspec.yaml 文件，这样它就可以提供必要的依赖项，并允许将这个包导入到 web 和移动项目中。

然后，我必须创建一个文件 **common_bloc_lyrics.dart，**，它将导出这个新包将提供的所有依赖项。

```
name: common_bloc_lyrics
description: Lyrics app shared Code between AngularDart and Flutter

version: 1.0.0+1

environment:
  sdk: ">=2.1.0 <3.0.0"

dependencies:
  meta: ^1.1.6
  equatable: ^0.2.0
  http: ^0.12.0
  bloc: ^2.0.0 
```

现在，我必须将这个包中的所有导入更改为新创建的文件的导入。

```
export 'src/model/song_base.dart';
export 'src/model/api/search_result_error.dart';
export 'src/model/api/search_result.dart';
export 'src/model/api/song_result.dart';
export 'src/model/api/artist.dart';

export 'src/repository/local_client.dart';
export 'src/repository/lyrics_repository.dart';
export 'src/repository/lyrics_client.dart';

export 'src/bloc/song/search/songs_search.dart';
export 'src/bloc/song/add_edit/song_add_edit.dart'; 
```

就这样。由于这些变化，共享库可以被 Flutter 和 AngularDart 项目导入。

```
import 'package:common_bloc_lyrics/common_bloc_lyrics.dart'; 
```

对 Flutter 应用程序的更改

## 由于所有的共享依赖项都是从移动应用程序项目中移走的，所以我必须修改我的 Flutter 项目的 pubspec.yaml 文件，以便它可以导入这些类，就像它只是另一个插件一样。

在那之后，我只需要将所有共享的类的导入更改为在公共包中创建的导出文件，类似于我之前所做的。每个使用 BLoC 或 models 的类都应该有这样的导入:

```
name: flutter_mobile_bloc_lyrics
description: Flutter BLoC shared code example

version: 1.0.0+1

environment:
  sdk: ">=2.1.0 <3.0.0"

dependencies:
  ...
  common_bloc_lyrics:
    path: ../common_bloc_lyrics 
```

就这样。由于这些改变，我已经设法使我的移动应用程序与共享代码包一起工作。

```
import 'package:common_bloc_lyrics/common_bloc_lyrics.dart'; 
```

角省道应用

## 为了创建 AngularDart 项目，我使用了推荐的 [Dart 项目生成器](http://web.archive.org/web/20221209114049/https://github.com/dart-lang/stagehand)，称为 Stagehand。下载之后，我还必须[更新我的系统路径](http://web.archive.org/web/20221209114049/https://stackoverflow.com/a/43765187/7120147)，这样它就会指向需要的工具。

使用 Stagehand 创建 AngularDart 应用程序与生成新的 Flutter 项目一样简单，您只需键入以下命令:

正如我之前所说的，我没有很多使用 web 应用程序的经验。我写的这个是基于 BLoC 库文档中的 [Github 搜索示例](http://web.archive.org/web/20221209114049/https://bloclibrary.dev/#/flutterangulargithubsearch)。

```
stagehand web-angular
```

由于这段代码与示例中的代码非常相似，所以我不会描述 web 项目本身。但是如果你对它是什么样子很好奇，你可以去看看它的[资源库](http://web.archive.org/web/20221209114049/https://github.com/kkogut95/bloc_lyrics_shared)。

要在浏览器中运行 AngularDart 项目，您需要执行以下命令:

To run AngularDart projects in your browser you need to execute this command:

```
webdev serve
```

![screencast 2019-11-28 16-57-31](img/951dc1d57f40ab99324915cecb00c4ad.png)

使用 AngularDart 与[用 Flutter](/web/20221209114049/https://www.netguru.com/blog/is-flutter-a-programming-language) 编写应用程序有很大不同。视图是用 HTML 编写的，Dart 文件为它们提供数据和类实例。

例如，项目的搜索栏 Dart 类如下所示:

它看起来类似于该组件的移动应用程序版本。由于有了 **@Input()** 注释，SongSearchBloC 被注入到这个类中。与移动版本类似，每次搜索栏中的输入发生变化时，都应该调用方法 **onTextChanged** 。 **templateUrl** 属性告诉我们应该使用哪个视图布局来创建这个组件。搜索栏 HTML 文件如下所示:

```
import 'package:angular/angular.dart';

import 'package:common_bloc_lyrics/common_bloc_lyrics.dart';

@Component(
  selector: 'search-bar',
  templateUrl: 'song_search_bar_component.html',
)
class SearchBarComponent {
  @Input()
  SongsSearchBloc songsSearchBloc;

  void onTextChanged(String text) {
    songsSearchBloc.add(TextChanged(query: text));
  }
} 
```

由于有了 **keyup** 关键字，在用户输入的每个字符上，都会调用 Dart 类中的函数 onTextChanges。

```
<label class="clip" for="term">Enter song title</label>
<input
  id="term"
  placeholder="Song title"
  class="input-reset outline-transparent glow o-50 bg-near-black near-white w-100 pv2 border-box b--white-50 br-0 bl-0 bt-0 bb-ridge mb3"
  autofocus
  (keyup)="onTextChanged($event.target.value)"
/> 
```

BLoC 注入的依赖项是在主搜索组件中创建的，与移动版本类似，它为搜索栏和列表创建视图实例。

在 **ngOnInit** 中初始化 BLoC 确保它的实例将在组件初始化时被创建，并且可以被这个类中创建的所有视图访问。每次视图被销毁，方法 **ngOnDestroy** 被调用。因此，这是关闭 BLoC 的最佳位置，因为它将不再被需要。

```
@Component(
    selector: 'search-form',
    templateUrl: 'search_song_component.html',
    directives: [
      SearchBarComponent,
      SearchBodyComponent
    ],
    pipes: [
      BlocPipe
    ])
class SearchSongComponent implements OnInit, OnDestroy {
  @Input()
  LyricsRepository lyricsRepository;

  SongsSearchBloc songsSearchBloc;

  @override
  void ngOnInit() {
    songsSearchBloc = SongsSearchBloc(
      lyricsRepository: lyricsRepository,
    );
  }

  @override
  void ngOnDestroy() {
    songsSearchBloc.close();
  }
} 
```

由于 web 应用程序有特殊的安全限制，值得指出的是，如果 web 服务器没有设置从应用程序所在的域接受请求的 **CORS** 头，那么您就不能向它发出请求。我用过的 API 没有这个功能，所以要运行我的 web 应用程序，我必须在没有 CORS 的情况下运行浏览器。当你在使用一个定制的 API 时，给它添加 CORS 头文件并不困难，但是你需要记住，你不能用大多数外部 API 做到这一点。

将应用程序转换为 Flutter web

## 创建一个共享的 web 和移动应用程序需要时间。它还需要一些关于省道、扑动和成角的知识。对于大多数移动开发者来说，这可能不是最好的解决方案。但是，如果你想把你的 Flutter 应用程序转换成 web 格式，有一个更简单的方法。

Flutter For Web 让您可以轻松地将任何应用程序转换为可工作的 Web 应用程序，无论您使用哪种架构。由于它仍处于预览阶段，因此不建议在生产就绪的应用程序中使用。

如果你想了解更多，我推荐这篇文章。通过遵循本文中描述的步骤，我成功地在浏览器上本地转换和运行了我的应用程序。

为了让我的应用程序工作，我必须在 Java 代码中做一些变通。您可以看到我是如何在库中的[提交中做到这一点的。](http://web.archive.org/web/20221209114049/https://github.com/kkogut95/flutter_web_bloc_lyrics/commits/master)

但是这些解决办法仍然没有让我的应用程序工作，我看到的只是一个白屏。我要做的下一件事是删除我用于应用程序翻译的库，因为它在 web 上不受支持。

But these workarounds still didn't make my app work and all I saw was a white screen. The next thing that I had to do was to remove the library that I used for app translations, since it was not supported on the web.

![screencast 2019-11-29 13-42-06](img/32a38e0eb9ef8bd4f7eb8197770f707d.png)

这些修复使我的应用程序能够工作，但并不是所有的东西都像它应该的那样运行。在移动应用版本中，当你滑动列表中的元素时，它会被移除。但是类似于用于翻译的库，用于滑动和删除的库在 web 项目上不受支持。

令我惊喜的是，集团图书馆发挥了应有的作用。web 应用程序中没有任何不同。

摘要

## 目前，移动开发者还没有简单的方法将他们的移动应用程序转换成生产就绪的网络应用程序。使用共享代码需要大量的知识，但它并没有得到很好的支持，因为 AngularDart 不像其他 web 技术那样流行。但是现在，最好的方法是用 Flutter 创建一个生产就绪的 web 应用程序。这是为 Flutter web 的成功祈祷的另一个原因。

照片由 [青奥会设计](http://web.archive.org/web/20221209114049/https://unsplash.com/@yogasdesign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 于 [Unsplash](http://web.archive.org/web/20221209114049/https://unsplash.com/s/photos/smartphone-laptop?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Photo by [Yogas Design](http://web.archive.org/web/20221209114049/https://unsplash.com/@yogasdesign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](http://web.archive.org/web/20221209114049/https://unsplash.com/s/photos/smartphone-laptop?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)