# 如何用另一个 CSS 类覆盖一个 CSS 类

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-to-override-one-css-class-with-another>

 如果你想添加一个自定义样式到一个已经被其他 css 类定义了样式的对象，你可能需要替换一些样式。以下是如何做到这一点:

Firstly, check which class overrides it.

假设您的标记返回:
`<div class="range-specific range"></div>`

### 如果你的 CSS 是这样定义的:

。范围特定的{ foo}

。范围{ bar}

...**靶场**将会胜出。

如果您将其更改为:

### 。range-specific . range { foo；}
。范围{ bar}
...特定的**会赢。**

如果你想做得更好，请这样做:

### 。range-specific . range-specific { foo；}
。范围{ bar}

和**具体的**一定会赢。

这样做更好:

。范围{ bar}
。范围特定的{ foo}

### It's Even Better to Do This:

.range { bar; }
.range-specific { foo; }