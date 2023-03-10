# 章节-装饰-android 简介

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/sections-decorator-android-introduction>

 你有没有想过要有一个滚动列表，其中的部分标题一直可见？现在你可以在自己的应用程序中完成。只需使用我们的小图书馆部分-装饰！

它支持 Recyclerview 的垂直和水平线性布局管理器，可以很容易地定制。您可以更改显示页眉的线条和文本视图的颜色和宽度。

## 怎么用？

1.  将 jitpack.io 添加到“build.gradle”项目存储库列表中:

    ```
    repositories {
       maven { url 'https://jitpack.io' }
    }
    ```

2.  将库依赖项添加到应用程序的模块 build.gradle 依赖项列表:

    ```
    implementation 'com.github.netguru:sections-decorator-android:0.1.1'
    ```

3.  在 RecyclerView 的适配器类中实现 SectionsAdapterInterface 接口，该接口告诉库这些节是如何构造的:

    ```
    interface SectionsAdapterInterface {
       fun getSectionsCount() : Int
       fun getSectionTitleAt(sectionIndex: Int): String
       fun getItemCountForSection(sectionIndex: Int): Int
    } 
    ```

    第一种方法返回 adapter 中可用的许多部分。第二个问题——每个部分的标题是什么，最后一个问题——每个部分有多少条目。

4.  向回收器视图添加 SectionDecorator。

    ```
    SectionDecorator decorator = new SectionDecorator(getContext());
    decorator.setLineColor(R.color.green)
    decorator.setLineWidth(15f)
    recyclerView.addItemDecoration(decorator);
    ```

就是这样:

总结

源代码在 Github 上有:[【https://github.com/netguru/sections-decorator-android】](http://web.archive.org/web/20221203101917/https://github.com/netguru/sections-decorator-android)

![](img/e61b503d893a20f62c0420eeb8a3d627.png)

你觉得怎么样？如果您有任何问题或反馈，请随时参与该项目并联系我们。我们很乐意帮助并听取您的意见！

## 照片由 [苏珊](http://web.archive.org/web/20221203101917/https://unsplash.com/photos/2JIvboGLeho?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [上](http://web.archive.org/web/20221203101917/https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

The source code is available on Github: [https://github.com/netguru/sections-decorator-android](http://web.archive.org/web/20221203101917/https://github.com/netguru/sections-decorator-android)

How do you like it? Feel free to contribute to the project and contact us if you have any questions or feedback. We are happy to help and listen to your opinion!

Photo by [Susan Yin](http://web.archive.org/web/20221203101917/https://unsplash.com/photos/2JIvboGLeho?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](http://web.archive.org/web/20221203101917/https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)