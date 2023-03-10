# 如何使用 Emberjs 设计认证

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-to-use-devise-authentication-with-emberjs>

 这里有一个使用 Ember 应用程序设置 Devise 的指南。Devise 是 Ruby 社区范围内的默认身份验证。Ember-simple-auth 是 Ember 社区范围内的默认身份验证。这两者配合得很好——使用它们，不要重新发明轮子。

## 后端设置

## 您应该使用什么:

设计 gem 作为认证基础

*   作为令牌处理程序的 simple _ token _ authentic ation-令牌认证支持已从 Devise 中移除；出于安全原因，你应该使用宝石
*   rack-cors 支持跨产地资源共享
*   步骤 1:设置您的模型

### 按照 simple_token 身份验证的[自述文件中的说明进行设置。记住运行迁移，将身份验证令牌添加到用户的表中。](http://web.archive.org/web/20220925090804/https://github.com/gonzalo-bulnes/simple_token_authentication)

[代码]

[代码]

步骤 2:设置基本控制器

确保在/api 命名空间和目录下限定控制器的范围。创建一些基本的控制器类，它将[作为用户](http://web.archive.org/web/20220925090804/https://github.com/gonzalo-bulnes/simple_token_authentication#allow-controllers-to-handle-token-authentication)的令牌认证处理程序。移除protect _ from _ forcer with::exception-它对 Ember 不起作用，因为表单不是由 Rails 呈现的。

### [代码]

步骤 3:设置会话控制器

您的会话控制器应该从device::session controller继承并覆盖 create action。在这个操作中，您返回将在前端作为会话数据处理的数据。您应该至少输入认证参数(电子邮件/令牌)。在那里，您可以添加任何必要的内容——如用户 id、角色、全名或配置文件 id。

[代码]

### 步骤 4:设置路线

在您的路由中使用device _ for来指定会话控制器:device _ for:用户，控制器:{ sessions: 'api/sessions' }

[代码]

步骤 5:禁用会话存储为 Cookie

### Rails 应用程序不应该发布会话 cookies。相反，身份验证应该专门通过身份验证令牌来完成。在 Rails 中禁用会话最简单的方法是添加一个初始化器配置/初始化器/session_store.rb 并禁用该文件中的会话存储。

[代码]

步骤 6:添加 CORS 中间件

### 将 CORS 中间件添加到您的 config/application.rb.

[代码]

[code]

前端设置

### 您应该使用什么:

成员简单认证用于基础认证和设备集成

来自的ApplicationRouteMixinember-simple-auth将处理认证动作

AuthenticatedRouteMixin 来自 ember-simple-auth ，如果用户未通过身份验证，它将重定向用户

## 来自 ember-simple-auth 的 UnauthenticatedRouteMixin ，如果用户通过了身份验证，它将重定向用户

来自的DataAdapterMixinember-simple-auth将授权 EmberData 请求

*   步骤 1:设置申请路线
*   从 ember-simple-auth 扩展基本应用程序路由 ApplicationRouteMixin 。
*   在app/pods/application/route . js
*   [代码]

步骤 2:创建登录路由

### 创建从 ember-simple-auth 扩展UnauthenticatedRouteMixin的登录路由。

app/pod/log in/controller . js

[代码]

app/pods/login/route.js

[代码]

### app/pods/log in/template . HBS

[代码]

app/router.js

[代码]

步骤 3:创建已登录的路由

该路由应该在所有必须限制已验证用户的路由之上。如有必要，可以将路径设置为空字符串。它应该从 ember-simple-auth 扩展 AuthenticatedRouteMixin 。

app/pods/已登录/route.js

[代码]

app/pods/已登录/template.hbs

[代码]

app/router.js

### [代码]

步骤 4:创建设备验证器

在app/授权码中创建名为device . js的文件。

[代码]

步骤 5:创建设计授权者

在app/授权人中创建名为device . js的文件。

[代码]

步骤 6:设置应用适配器以授权会员数据

### 在 app/adapters/ 中创建 application.js 适配器。

[代码]

步骤 7:设置会话服务

在应用/服务中创建 session.js 文件，该文件将从 ember-simple-auth 会话服务继承。你可以在这里放一些计算属性或者 currentUser 方法。

步骤 8:设置您的环境

在内容安全策略中更新您的 connect-src 密钥。为 ember-simple-auth 配置对象中的 routeAfterAuthentication 和routeIfAlreadyAuthenticated密钥添加路由名称。

[代码]

### 摘要

用 Emberjs 设置 Devise 不是一个 5 分钟的任务，但也没那么难。如果有什么问题，欢迎在下面评论。

[code]

### Step 7: Setup Your Session Service

Create session.js file in app/services that will inherit from ember-simple-auth session service. You can put here some computed properties or currentUser method.

### Step 8: Setup Your Environment

Update your connect-src key in contentSecurityPolicy. Add route names for routeAfterAuthentication and routeIfAlreadyAuthenticated keys in ember-simple-auth config object.

[code]

### Summary

Setting up Devise with Emberjs is not a 5-minute task, but it's not that hard. If you have any problems, feel free to comment below.