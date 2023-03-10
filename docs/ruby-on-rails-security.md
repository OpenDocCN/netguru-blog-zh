# Ruby on Rails 安全指南(教程)

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/ruby-on-rails-security>

 安全性是任何 web 应用程序最重要的方面之一。Ruby on Rails 真正酷的地方在于它提供了很好的设置和技术来抵御大多数攻击。

让我们重点关注三种类型的攻击和对策。为了让它更有趣，我们将在一个实际的应用程序中检查它们，我们还将看看如果我们决定变得脆弱并被黑客攻击会发生什么。

## Ruby on Rails 安全吗？

像任何框架一样，这个问题的答案是复杂的。当[正确使用 Rails 时，你可以构建安全的应用](/web/20220924154621/https://www.netguru.com/services/ruby-on-rails-development),因为它在核心功能中内置了许多特性。然而，安全性不仅仅是使用一个给定的框架，它主要取决于开发人员使用框架和开发方法。让我们深入一些例子。

### 示例应用程序

克隆存储库[](http://web.archive.org/web/20220924154621/https://github.com/mateuszpalyz/rails_security)，并遵循自述文件中的设置指南。

现在，在[http://localhost:3001](http://web.archive.org/web/20220924154621/http://localhost:3001/)我们有一个样本网站，我们可以在那里开始黑客攻击。

### CSRF

正如 Rails guides [所说的](http://web.archive.org/web/20220924154621/https://guides.rubyonrails.org/security.html#cross-site-request-forgery-csrf)，跨站点请求伪造是一种攻击,“其工作原理是在访问 web 应用程序的页面中包含恶意代码或链接，用户被认为已经验证了该应用程序。如果该 web 应用程序的会话没有超时，攻击者可能会执行未经授权的命令。”

那么所有这些实际上意味着什么呢？假设我们有一个控制器，它实现了一个删除给定资源的方法。如果攻击者创建了一个使用该方法的链接，并说服我们点击该链接，例如来自电子邮件的链接，会发生什么？在这种情况下，Rails 默认设置生效，我们将得到一个 CSRF 令牌不匹配。该令牌包含在请求中，然后在服务器端进行验证。

当然，我们可以关闭这种保护。实际上，在没有更深入了解自己在做什么的情况下玩“skip _ before _ action:verify _ authentic ity _ token”是一个相当普遍的问题。

我们来看一个活生生的例子，访问[http://localhost:3001/csrf](http://web.archive.org/web/20220924154621/http://localhost:3001/csrf)。这个页面显示了一些来自数据库的记录，我们也有一个链接发送一封由攻击者准备的电子邮件。让我们点击这个链接(由于 letter_opener gem，它将在浏览器中打开)。在邮件中，我们有两个链接，一个用于销毁使用默认设置并打开 CSRF 保护的操作，另一个用于跳过 CSRF 保护:

```
 protect_from_forgery except: :destroy_csrf_off

  def destroy_csrf_off
    destroy
  end

  def destroy_csrf_on
    destroy
  end

  def destroy
    @some_record = SomeRecord.find(params[:id])
    @some_record.destroy

    redirect_to csrf_path
  end 
```

来自[https://github . com/mateuspallyz/rails _ security/blob/master/app/controllers/pages _ controller . Rb](http://web.archive.org/web/20220924154621/https://github.com/mateuszpalyz/rails_security/blob/master/app/controllers/pages_controller.rb)

当我们单击安全链接时，我们得到一个异常，但当我们单击另一个链接时，some_records 表中的一条记录被删除。😨

### **跨站点脚本(XSS)**

再次引用关于跨站点脚本的 [Rails 指南](http://web.archive.org/web/20220924154621/https://guides.rubyonrails.org/security.html#cross-site-scripting-xss)，“这种恶意攻击注入客户端可执行代码。”攻击者如何注入这些代码？基本上，任何有用户输入的地方都可能是易受攻击的。最常见的 XSS 发生在 WYSIWYG 编辑器中，它不能正确地转义用户提交的输入。当代码被注入时，攻击者可以运行他们能想到的任何恶意代码，例如窃取 cookies。通常，我们谈论的是 JavaScript 注入，但 CSS 也容易受到攻击。Rails 通过用户输入杀毒软件保护我们免受这种攻击。让我们看看实际情况。访问[http://localhost:3001/XSS](http://web.archive.org/web/20220924154621/http://localhost:3001/xss)，在这里我们有一个潜在危险的字符串:

```
<script>alert('oh no, xss :(')</script>
```

The first link leads to a page where this string is not escaped:

```
<%=  "<script>alert('oh no, xss :(')</script>".html_safe  %>
```

来自[https://github . com/mateuzpallyz/rails _ security/blob/master/app/views/pages/XSS _ vulnerable . html . erb # L15](http://web.archive.org/web/20220924154621/https://github.com/mateuszpalyz/rails_security/blob/master/app/views/pages/xss_vulnerable.html.erb#L15)

这会导致恶意代码被执行。

第二个链接指向一个页面，其中字符串被转义并简单地显示，但不执行任何代码。

```
<%= "<script>alert('oh no, xss :(')</script>" %>
```

来自[https://github . com/mateuspalyz/rails _ security/blob/master/app/views/pages/XSS _ free . html . erb # L15](http://web.archive.org/web/20220924154621/https://github.com/mateuszpalyz/rails_security/blob/master/app/views/pages/xss_free.html.erb#L15)

### SQL 注入

[《Rails 指南》警告](http://web.archive.org/web/20220924154621/https://guides.rubyonrails.org/security.html#sql-injection) ，“SQL 注入攻击旨在通过操纵 web 应用程序参数来影响数据库查询。SQL 注入攻击的一个常见目标是绕过授权。另一个目标是进行数据操作或读取任意数据。”这是一个严重的问题，不仅会危及单个用户的数据，还会危及包含任何敏感数据的整个数据库。让我们来看一个例子。

访问[http://localhost:3001/SQL _ injection](http://web.archive.org/web/20220924154621/http://localhost:3001/sql_injection)。这里，我们有一个页面显示 some_records 表中的记录(名称和优先级)，下面有一个搜索输入，不幸的是，它容易受到注入。

```
@some_records = SomeRecord.where("name like '%#{params.dig(:query, :name)}%'") # with sql injection
```

来自[https://github . com/mateuspalyz/rails _ security/blob/master/app/controllers/pages _ controller . Rb # L39](http://web.archive.org/web/20220924154621/https://github.com/mateuszpalyz/rails_security/blob/master/app/controllers/pages_controller.rb#L39)

我们将用户输入直接插入到查询中。如果我们使用

```
@some_records = SomeRecord.where('name like ?', "%#{params.dig(:query, :name)}%") # without sql injection
```

from[https://github . com/mateuszpalyz/rails _ security/blob/master/app/controllers/pages _ controller . Rb # L38](http://web.archive.org/web/20220924154621/https://github.com/mateuszpalyz/rails_security/blob/master/app/controllers/pages_controller.rb#L38)它会避开用户输入并使查询安全(您可以稍后通过取消注释该行并注释掉 L39 来测试这一点)。

现在让我们来点刺激的，我们可以删除整个 some_records 表，但是从其他表中获取数据会更有趣。这里有一些线索:这里可以使用 union select，另一个表叫做 some_other_records，它的列数与 some_records 相同-> 5。黑客快乐！如果你需要更多的帮助，一个示例解决方案隐藏在页面的底部，只需将其复制到搜索输入中。

### 摘要

像往常一样，有了 Rails，我们就有了一个坚实的基础，可以保证我们的应用程序安全。重要的是要知道可能会进行什么样的攻击，有什么对策，而不是用 Rails 提供的现成的东西来对抗。这样我们就可以构建安全的应用程序。注意安全。