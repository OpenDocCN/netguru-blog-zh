# 数组中的新元素显示在底部，并在短暂延迟后显示在顶部

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/new-elements-in-array-are-shown-at-the-bottom-and-go-to-the-top-after-short-delay>

 假设您有一个有序笔记列表...型号:

```
sortCreatedAtDesc: ['createdAt:desc'],
notesSorted: Ember.computed.sort('notes', 'sortCreatedAtDesc')
```

...它们显示在这样的视图中:

问题

## 向收藏添加新笔记后，它会出现在页面的底部，并在短暂的延迟后移动到顶部(它应该出现在第一个位置，因为它是最新的)。

如何让它立刻出现在最上面？

解决办法

显然，发生这种情况是因为新便笺在信息从服务器返回之前就已显示。属性是在后端设置的，所以在很短的时间内——当等待后端响应时——它的值为 null 并被放置在页面的底部。

这个问题的解决方案是只对保存的笔记进行排序，省略掉那些带有是新的属性的笔记。
型号中的
:

## 请记住，在点击“保存”和在列表上显示新注释之间会有短暂的延迟。因此，您应该考虑相应地更改 UI。例如，您可以禁用“保存”按钮或显示“笔记正在保存中…”给用户的消息。

```
sortCreatedAtDesc: ['createdAt:desc'],
notesSaved: Ember.computed.filterBy('notes', 'isNew', false),
notesSorted: Ember.computed.sort('notesSaved', 'sortCreatedAtDesc'),
```

下面是在保存操作中使用保存的示例:

Here's an exemplary use of isSaving in save action:

```
saveNote: function() {
 this.set('isSaving', true);
 //...
 note.save().then(() => {
   this.set('isSaving', false);
 }
 this.set("noteBody", "");
}
```