# 如何用偏执狂 Gem 配置 Ruby on Rails

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-to-configure-ruby-on-rails-with-paranoia-gem>

 偏执狂是一块宝石，它能让你隐藏记录，而不是毁掉记录。如果需要，您可以稍后恢复它们。这是一个非常简单的 actsaspaonical 的重新实现(一个包含大约 300 行代码的文件)。

我假设您有一个 Rails 应用程序，它创建了用户、对话和消息模型，并在对话中添加了显示作者和消息的基本视图。 

你将使用什么

## What You Will Use

优点:

### 非常简单的宝石，

*   易于调试(只有一个文件)，

*   它做它设定要做的事，

*   积极维护，

*   文档不错。

*   缺点:

### 第一步

### 将偏执狂宝石添加到您的宝石文件中:

您这样添加它是因为最新的版本不包括本教程中将要用到的一些特性。

```
gem 'paranoia', git: 'git@github.com:rubysherpas/paranoia.git', branch: 'core'
```

第二步

现在，假设您想要删除一个用户，但是他的消息仍然应该显示他的作者姓名(在您的视图中，您将看到类似 message.user.name 的内容)。如果您删除了那个用户，当您试图显示他的消息时，会得到一个错误:

### 为了避免这种情况，您应该将记录标记为已删除，而不是销毁记录。这叫做软删除，这就是偏执狂宝石的全部。

```
undefined method `name` for nil:NilClass
```

要为用户模型启用这种行为，请向其添加以下代码行:

您还需要生成并运行这个迁移:

从现在开始，对任何用户调用 destroy 都会将 deleted_at 设置为当前日期，而不是实际销毁记录。

```
class User < ActiveRecord::Base
  acts_as_paranoid
  [...]
```

第三步

```
rails generate migration AddDeletedAtToUsers deleted_at:datetime:index
rake db:migrate
```

但是，如果您尝试轻轻地删除一个用户并显示他的消息，您仍然会得到和以前一样的错误。那是因为偏执狂 gem 改变了一个模型的 default_scope，所以每个查询都会把软删除的记录当做不存在(WHERE“users”)。“deleted_at”为空将被添加到查询中)。当然，如果您喜欢您的 default_scope，并且不希望它发生变化，您可以做到这一点:

您还可以通过其他方式访问被软删除的用户记录:

### 如果您需要包括或忽略其他地方被软删除的记录，除了上述方法之外，还有几种方法可以做到这一点:

User.with_deleted 、user . with _ deleted和 User.only_deleted 范围

```
# not available in the newest release, you have to add paranoia gem as in step 1
acts_as_paranoid without_default_scope: true
```

可以用 user.paranoia_destroyed 检查一条记录是否被软删除？还是 user.deleted？方法

请注意，如果您希望索引忽略被软删除的记录，您必须按如下方式修改它们:

*   第四步

*   如果要恢复用户，请使用以下代码:

或者

```
add_index :users, :some_column, where: "deleted_at IS NULL"
```

### 第五步

如果你想轻轻地删除一个用户和他的所有邮件呢？您可以简单地通过为消息模型实现 acts_as_paranoid (如步骤 2 中一样，您还需要添加一个迁移)，然后使用 dependent: :destroy 来实现

```
User.restore(message.user.id)
```

```
Message.user.restore
```

### 请注意，如果您没有为消息模型实现 acts_as_paranoid ，消息将被正常销毁，并且您将无法恢复它们。此外，恢复用户不会恢复他的所有邮件，除非您使用:

或者

```
class User < ActiveRecord::Base 
  acts_as_paranoid 
  has_many :messages, dependent: :destroy 
  [...]
```

摘要

偏执狂是一个非常简单的宝石，使软删除更容易。如果您需要这样一个简单的功能，并且不需要实现您自己的解决方案所带来的更好的控制，那么选择 paranoia 应该会非常有效。

```
User.restore(user_id, recursive: true)
```

or

```
user.restore(recursive: true)
```

### Summary

Paranoia is a really simple gem that makes soft deleting easier. If you need such a simple functionality and you don’t need a better control that comes with implementing your own solution, choosing paranoia should work very well.