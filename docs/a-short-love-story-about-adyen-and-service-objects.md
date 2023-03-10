# 关于阿丁和服务对象的爱情小故事

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/a-short-love-story-about-adyen-and-service-objects>

 一段时间以前，我被分配到一个神秘的任务，为我们客户的奥地利服务分析和实现与一个新的支付提供商的集成。要求很简单:新的支付服务应该是 Adyen，因为 Adyen 支持 SEPA 支付。这是一个关于这一切是如何发生的故事。

等等，什么？

## **TL；T2 博士:下面是关于这个项目和问题的一些信息。对于更多的技术资料，只需向下滚动，直到你看到固定宽度字体的文本。**

**项目**

### 我实现这个功能的项目叫做[](http://web.archive.org/web/20221202095656/http://lemonfrog.ch/)。它是一套针对 *配对用户* 的 web apps，不仅仅是约会意义上的。我们可以将 Lemonfrog 的服务分为两大类:

**护理服务** :他们为找工作的人和寻求帮助的人牵线搭桥，如 [tutor24](http://web.archive.org/web/20221202095656/http://tutor24.ch/) (学生和家教)或 [家政服务 24](http://web.archive.org/web/20221202095656/https://homeservice24.ch/) (各种家务)。

*   **交友服务** ，如[singlimitkind](http://web.archive.org/web/20221202095656/https://singlemitkind.ch/)(针对单亲)。
*   虽然 Lemonfrog 的总部设在瑞士，但它的服务也在奥地利和德国，这两个国家也有大量的用户。

我们客户的主要收入来源是高级账户——某些类型的用户需要高级账户才能与其他人联系。各种各样的服务及其目标用户需要不止一种支付方式，因此除了最受欢迎的支付提供商(即 Stripe 和 PayPal)，我们还有其他选择，如 *通过电子邮件/信函开具发票* 。记住，不是每个人都知道如何使用信用卡。

**问题**

### 对于一个以瑞士为基础的以奥地利为目标的应用程序来说，最大的问题是奥地利货币是欧元，而瑞士货币是法郎。这个问题的解决方案被称为 **单一欧元支付区** ，这是欧盟为简化以欧元计价的银行转账而提出的支付整合倡议。

更简单地说，由于 SEPA(奥地利和瑞士都是其成员),奥地利人可以很容易地向瑞士人付款，而且费用更低。

Adyen

### Adyen 是最符合支持 SEPA 要求的支付解决方案提供商。它提供了世界各地的多种支付方式(包括特定于一个国家的方式和更多的全球方式，如 SEPA 银行转账)。

**如何？**

## 好了，现在我们知道了问题是什么，我们应该使用什么工具，是时候分析如何使用这个工具来解决问题了。经过几个小时的研究，我发现只有一个现成的解决方案(名为 adyen 的 gem)可能会有所帮助，但是考虑到我们的需求以及该 gem 似乎没有得到很好的支持这一事实(上次提交是在将近半年前)，我们决定最好编写自己的解决方案。

没有必要深入研究 Adyen 的细节，它提供什么或如何工作——所有这些信息都可以在 [开发者文档](http://web.archive.org/web/20221202095656/https://docs.adyen.com/) 中找到。在这篇文章中，我将从开发者的角度出发，只关注一些关于如何在你的应用中实现与 Adyen 集成的建议。

在我工作的“调查”阶段，我试图写下所有必要的信息:

我们需要为我们的每个服务存储三个值: **帐户名** ， **皮肤代码** 和 **HMAC 键** 。

*   支付流程可以分为三个主要动作:
*   创建并发送 **付款请求** 。

*   接收并处理用户完成支付后返回我们服务时的 **响应** 。
*   经办 **通知** 关于支付状态的变更。
*   创建和发送付款请求包括准备正确的 **请求参数** ，这些参数符合 Adyen 的文档以及我们请求的 **签名** 。

*   我决定将每个动作分成单独的服务类——一个服务对象负责一个动作。如果你想了解更多的服务对象，可以在这里 找到一篇关于这种做法的文章 [。](http://web.archive.org/web/20221202095656/https://www.netguru.com/blog/service-objects-in-rails)

**充电请求**

### 要创建并成功向 Adyen 发送收费请求，我们需要执行三个步骤:

根据支付数据建立参数列表。

*   签署这些参数。
*   基于这些参数和应用程序当前运行的环境构建请求 URI。
*   从我们支付管理员的角度来看，我们只需要在选择 Adyen 支付方式时将用户重定向到的 URI。这将由 负责请求服务 : 处理

它的构造函数接受两个对象作为参数:一个是支付数据，另一个是与当前服务及其配置相关的数据。然后，它将构建参数委托给我们的RequestParamsService。基于 @params hash 和当前 app 环境，构建并返回一个 URI 给 Adyen 的 live 或 test 端点。

```
 module Adyen
  class ChargeRequestService
    attr_reader :params

    def initialize(payment, service)
      @params = RequestParamsService.new(payment, service).call
    end

    def call
      request_url + "?" + URI.encode_www_form(params)
    end

    private

    def env
      Rails.env.production? ? "live" : "test"
    end

    def request_url
      "https://#{env}.adyen.com/hpp/select.shtml"
    end
  end
end 
```

**构建和签名参数**

### 现在，我们将仔细看看参数是如何构建和签名的。

在负责构建我们的参数的服务中，我们使用了一些常量中定义的值。最重要的一个叫做 参数 ，因为它包含了我们希望包含在生成的 hash 中的所有参数的列表(按字母顺序排列)。名字写在 snake_case、 中，因为每个名字都有自己的私有方法负责生成它的值，但是 Adyen 要求我们的键写在lower case中。这就是为什么我们需要叫 。camelize(:lower) 在我们的 build_params 方法中。

```
 module Adyen
  class RequestParamsService
    PARAMETERS = %w(... merchant_account ...).freeze

    attr_reader :params

    def initialize(payment, service)
      @service = service
      @payment = payment
      @params = build_params
    end

    def call
      params["merchantSig"] = build_signature
      params
    end

    private

    attr_reader :service, :payment

    def build_params
      PARAMETERS.each_with_object({}) { |param, hash| hash[param.camelize(:lower)] = send(param) }
    end

    def build_signature
      SignatureService.new(service.adyen_hmac_key, params).call
    end

    def merchant_account
      service.adyen_account_name || GLOBAL_ADYEN_ACCOUNT
    end
  end
end 
```

**参数签名**

### 在返回带有参数的 hash 之前，我们想通过调用signature reservice来生成他们的签名，HMAC 密钥是通过 Adyen 面板生成的，并保存为 adyen_hmac_key 。

根据文档，签名生成算法应该是这样的:

```
 module Adyen
  class SignatureService
    def initialize(key, params)
      @key = Array(key).pack("H*")
      @params = params
    end

    def call
      calculate_signature
    end

    private

    attr_reader :key, :params

    def calculate_signature
      Base64.strict_encode64(hmac)
    end

    def hmac
      digest = OpenSSL::Digest.new("sha256")
      OpenSSL::HMAC.digest(digest, key, params_string)
    end

    def keys_string
      params.keys.join(":")
    end

    def params_string
      "#{keys_string}:#{values_string}"
    end

    def values_string
      params.values.map { |v| v.gsub(":", "\\:").gsub("\\", "\\\\") }.join(":")
    end
  end
end 
```

按关键字的字母顺序对哈希进行排序。

1.  分别用 \: 和 \\ 对值中的冒号和反斜杠进行转义。
2.  用 : 连接键，然后对值做同样的操作，并连接两个字符串(也用 : )。
3.  将 HMAC 密钥转换成二进制形式——它被认为是十六进制值。
4.  使用 SHA-256 计算第 3 步签名字符串的 HMAC。
5.  使用 Base64 编码方案对结果进行编码。
6.  Adyen 在其仪表板上提供了一个表格，用于测试生成的签名对于给定值是否正确。

**处理 Adyen 的响应**

### 付款完成后，Adyen 会将用户重定向回我们的页面，并向我们发送一些关于处理结果的信息。我们将在单独的服务中处理这些响应。

首先，我们希望根据收到的数据在数据库中找到付款。下一步是更新此付款的状态。我们还想保存付款的外部 ID——这可能对识别付款很有用。

```
 module Adyen
  class HandleResponseService
    attr_reader :params, :payment

    def initialize(params)
      @params = params
      @payment = Payment.find(payment_id)
    end

    def call
      payment.update_attributes(
        state: fetch_state,
        external_id: fetch_external_id
      )
    end

    private

    def fetch_external_id
      params["pspReference"]
    end

    def fetch_state
      params["authResult"]
    end

    def payment_id
      # Here you can get payment_id for example from “merchantReference”
    end
  end
end 
```

**响应变更:处理通知**

### SEPA 支付没有信用卡或 PayPal 支付快——在 SEPA 中，付款需要大约一个工作日的授权和结算时间。这使得接收和响应来自 Adyen 的通知对我们的案例至关重要。这是负责此事的服务。

Adyen 通知以一组notification items的形式出现——我们希望遍历每一项并处理每一个通知事件。这是在handle _ notification _ item方法中完成的。首先，我们需要检查是否为给定的数据找到了付款。接下来，我们应该检查通知是否说事件是成功的。如果不成功，我们可以以某种方式存储关于失败的数据(例如，将其发送到 Rollbar)。

```
 module Adyen
  class HandleNotificationService
    attr_reader :notifications

    def initialize(params)
      @notifications = params["notificationItems"]
    end

    def call
      notifications.each do |item|
        @current_item = item["NotificationRequestItem"]
        handle_notification_item
      end
    end

    private

    attr_reader :current_item

    def handle_notification_item
      return false unless payment
      if successful?
        send "handle_#{event}"
      else
        Rollbar.info(failure_reason_message)
      end
    rescue ::NoMethodError
      Rollbar.info(not_implemented_for_message)
    end

    def event
      current_item["eventCode"].downcase
    end

    def handle_authorisation
      # Code for handling authorisation event
    end

    def handle_cancellation
      # Code for handling cancellation event
    end

    def successful?
      current_item["success"] == "true"
    end
  end
end 
```

处理事件由 handle_【事件名称】 方法管理，这些方法是动态调用的——在这些方法中，我们可以实现负责在这种特定事件的情况下做我们想做的事情的代码(例如，对于handle _ authorization，我们可以将 支付 状态更新为 已支付 ，并触发一个邮件程序，通知用户支付已被批准我们还想存储关于未知事件的信息(Adyen 可能会发送大量事件代码，但只有少数对我们有用)——在NoMethodError的情况下，我们将信息发送到 Rollbar。

**总结**

## 从商业角度来看，Adyen 对你的客户非常有用。知道有这样的工具存在，知道怎么用就好。不幸的是，既没有面向 Ruby 开发者的文档，也没有最新的现成解决方案。希望这篇文章能帮助你在应用程序中实现与 Adyen 的集成。

Adyen can be very useful for your client from the business perspective. It’s good to know that such a tool exists and know how to use it. Unfortunately, there is neither documentation meant for Ruby developers nor any up-to-date out-of-the-box solution. Hopefully, this article will help you with implementing the integration with Adyen in your app.