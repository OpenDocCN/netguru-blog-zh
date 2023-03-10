# 如何使用 RSpec 模拟改进 Ruby 测试

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/ruby-tests-rspec-mocks>

 本文描述了 RSpec 环境中 mocks 的应用，RS pec 环境是领先的 [Ruby on Rails](/web/20220924171519/https://www.netguru.com/blog/is-ruby-on-rails-dead) 测试框架之一。Mocking 技术促进了编写单元测试的过程，对于那些想要将测试驱动开发集成到他们的软件开发过程中的人来说，它可能是有意义的。这项工作的目标是提供一个 RSpec 测试中模拟技术的简明摘要，以及代表典型用例的代码示例

## 什么是嘲讽？

模仿是指使用假对象来模仿真实应用程序组件的行为。因为模拟对象模拟原始组件的行为，所以可以在测试独立的应用程序代码时使用它们来处理与应用程序其他部分的交互。

### 嘲笑的好处

*   **测试环境构建过程的简化。构建返回特定值的对象通常比初始化和配置应用程序对象更容易，以便它们以特定的方式响应来自测试代码的消息。**
*   最小化时间和资源占用。被测代码可能会触发耗时耗资源的操作，比如访问数据库。模拟对象可以检测对目标组件的调用，并以指定的方式响应，而不需要实际执行任何繁重的工作。

### 模拟应用的主要驱动因素

mocks 应用程序的主要驱动因素是测试驱动的开发和应用程序及其外部组件之间的测试接口。请参见下面的描述:

#### 测试驱动开发

TDD 是一个连续的循环，为一个新的尚未实现的特性构建测试，实现特性，然后可选地重构测试和特性。

TDD 倾向于构建针对新实现的功能的小单元测试。广泛的单元测试集合允许快速识别错误的来源，并极大地增强了应用程序的重构。

TDD 的额外好处是，预先创建测试有助于从公共接口的角度考虑新功能，并有助于构建松散耦合的代码。模拟允许达到为专门针对新功能的测试构建环境的目标。

#### 测试应用程序及其外部组件之间的接口

通常，应用程序依赖于其存储库之外的代码，例如，提供发送电子邮件、跟踪网站流量等功能的外部服务。

执行包含应用程序和外部服务的系统测试可能不切实际，这是由于诸如显著的时间延迟、访问服务的高成本等因素。在这些情况下，模拟将模拟外部组件的工作。

### 模仿对象类型

各种各样的名字被用作模仿对象，这可能会导致混淆。杰拉德·梅萨罗什建议根据嘲笑对象的行为方式对其进行分类。他使用术语 double 作为包含模仿对象的所有变体的通用术语。特定的双变异由[马丁·福勒](http://web.archive.org/web/20220924171519/https://martinfowler.com/articles/mocksArentStubs.html)、[米隆·马斯顿和伊恩·迪斯](http://web.archive.org/web/20220924171519/https://pragprog.com/titles/rspec3/effective-testing-with-rspec-3/)总结:

*   **存根**–返回预编程的结果以响应特定的消息。
*   **Mock**–对它将收到的消息有内置的预期，如果这些预期没有实现，它就会失败。
*   **空对象**–返回自身以响应任何消息。
*   **Spy**–登记所有消息
*   **Fake**–一个对象的简化版本，在开发中可以正常运行，但不适合生产，例如 SQLite 数据库。
*   **Dummy**–一个被传来传去却从未被使用过的物体。

## 什么是 RSpec doubles？

RSpec 延续了梅萨罗什建议的命名惯例，使用 double 的概念来表示一个代表任何类型模仿对象的通用对象。

此外，RSpec 引入了自己的 double 类型划分，规定了可以测试哪些功能。这种划分独立于角色的划分，例如，每个 RSpec double 类型都可以充当一个 mock 或 stub 的角色。

### 三种类型的 RSpec double

这里有三种类型的 RSpec double:纯 double、验证 double、部分 double。

#### 1.纯双(也称为双或正常双)

纯 double 只能接收允许或预期的消息，如果接收到其他消息，将会导致错误。它决不会受到它要模拟的实际对象的约束。

#### 2.验证双精度

它是 pure double 的一种变体，带有一个附加约束，即它的消息是根据特定对象的实际实现来验证的。

换句话说，可以允许或期望验证 double 只接收那些在实际对象中实际实现的消息。

好处是基于验证 doubles 的测试用例不会因为测试不存在的方法而导致假阳性。

#### 3.部分双精度

它是实际对象和测试对象的混合体。实际对象扩展了允许和/或预期消息的功能。

部分双精度的优势在于，与纯双精度或验证双精度不同，它可以访问实际的实现，并且可以返回原始值或修改原始值(有关更多详细信息，请参见方法[和 _call_original](http://web.archive.org/web/20220924171519/https://relishapp.com/rspec/rspec-mocks/docs/configuring-responses/calling-the-original-implementation) 、[和 _wrap_original](http://web.archive.org/web/20220924171519/https://relishapp.com/rspec/rspec-mocks/v/3-10/docs/configuring-responses/wrapping-the-original-implementation) )。

此外，与纯双精度和验证双精度不同，部分双精度并不严格，也就是说，只要实现了方法，就可以调用既不允许也不期望的方法。

### 与纯替身一起工作

通过允许纯 double 接收消息并返回特定值作为响应，可以实现存根。在下面的例子中，创建了一个名为`random`的纯 double。它的目标是模拟随机实例的行为。每当收到`rand`消息时，它将返回值 7。

```
random_double = double('random')
allow(random_double).to receive(:rand).and_return(7)
expect((1..6).map{ random_double.rand(1..10) }).to eq([7,7,7,7,7,7])
```

存根可以扩展为返回特定的值序列。继续前面的例子，指示`random` double 返回斐波纳契数列中的 6 个数字。

```
random_double = double('random')
allow(random_double).to receive(:rand).and_return(1,1,2,3,5,8)
expect((1..6).map{ random_double.rand(1..10) }).to eq([1,1,2,3,5,8])
```

返回值序列可以通过使用块的任意复杂算法来定义。在下面的代码中，`rand` message 将返回由 range 参数指定的最小值。如果没有传递范围参数，它将返回数字 7。

```
random_double = double('random')
allow(random_double).to receive(:rand) do |arg|
  (arg && (arg.is_a? Range)) ? arg.min : 7
end
expect(random_double.rand).to eq(7)
expect(random_double.rand(1..10)).to eq(1)
expect(random_double.rand(11..20)).to eq(11)
```

嘲讽可以通过期待纯双精度接收消息来实现。对于消息参数和消息发送的次数，这种期望可以扩展。在下面的例子中，`random` double 需要接收 6 次带有参数的`rand`消息，该参数是`Range`的实例。

```
random_double = double('random')
expect(random_double).to receive(:rand).with(instance_of(Range))
                                       .exactly(6).times
6.times { random_double.rand(1..10) } 
```

### 使用验证双精度

验证双精度不同于纯双精度，因为它们是用`instance_double`、`class_double`或`object_double`而不是`double`来定义的。

*   方法`instance_double`接收一个类作为参数，它返回一个验证双精度值，该值只能接收那些在类中作为实例方法实现的消息。
*   方法`class_double`接收一个类作为参数，它返回一个验证双精度值，该值只能接收那些在类中作为类方法实现的消息。
*   方法`object_double`接收一个对象作为参数，并返回一个验证双精度值，该值只能接收那些在对象中实现的消息。

下面介绍了使用验证 doubles 来存根化实例方法。`Random`类的双存根`rand`实例方法。

```
random_double = instance_double(Random)
allow(random_double).to receive(:rand).and_return(7)
expect((1..6).map{ random_double.rand(1..10) }).to eq([7,7,7,7,7,7])
```

下面展示了使用验证 double 模拟实例方法。双模拟`Random`类的`rand`实例方法。

```
random_double = instance_double(Random)
expect(random_double).to receive(:rand).with(instance_of(Range))
                                       .exactly(6).times
6.times { random_double.rand(1..10) }
```

使用验证 doubles 的好处是，试图发送未在验证类中实现的消息会导致错误。在下面的例子中，向验证 double 发送`random`消息将产生一个错误，显示如下消息:“random 类没有实现 instance 方法:Random”。如果用`double`来代替，测试就通过了。

```
# This will cause error!
# random_double = instance_double(Random)
# allow(random_double).to receive(:random).and_return(7)
# random_double.random(1..10)
```

下面介绍了使用验证 doubles 的 Stubbing 类方法。双存根 `rand` 类 `Random` 类的方法。

```
random_double = class_double(Random)
allow(random_double).to receive(:rand).and_return(1,1,2,3,5,8)
expect((1..6).map{ random_double.rand(1..10) }).to eq([1,1,2,3,5,8])
```

如果验证 double 试图存根一个不存在的类方法，测试将失败。继续前面的例子，尝试存根`random`类方法将产生下面的错误消息:“random 类没有实现类方法:Random”。

```
# This will cause error!
# random_double = class_double(Random)
# allow(random_double).to receive(:random).and_return(1,1,2,3,5,8)
# expect((1..6).map{ random_double.random(1..10) }).to eq([1,1,2,3,5,8])
```

### 使用部分双精度

与纯双精度和验证双精度的情况不同，在部分双精度的情况下，不返回对结果双精度的引用。相反，底层类的行为被修改为包括存根和模仿功能。

可以使用`allow_any_instance_of`方法将特定的类作为参数来实现类实例的存根化。在下面的例子中，`Rand`类的所有实例将返回数字 7 来响应`rand`消息。

```
allow_any_instance_of(Random).to receive(:rand).and_return(7)
expect((1..6).map{ Random.new.rand(1..10) }).to eq([7,7,7,7,7,7])
```

任意复杂的存根算法可以通过传递一个块来描述，就像纯 doubless 和验证 double 的情况一样。更有趣的是，不像纯双精度和验证双精度，部分双精度允许算法基于使用`and_wrap_original`方法的原始实现。

在下面的例子中，如果实例方法`rand`接收一个`Range`作为参数，它将返回原始值，否则它将返回数字 7。

```
allow_any_instance_of(Random).to receive(:rand).and_wrap_original do |rand_method, arg|
  (arg && (arg.is_a? Range)) ? rand_method.call(arg) : 7
end
expect(Random.new.rand).to eq(7)
expect(Random.new.rand(1..10)).to be_between(1, 10)
expect(Random.new.rand(11..20)).to be_between(11, 20)
```

使用部分 double 模仿单个类实例可以通过使用`expect_any_instance_of`方法来实现。这种方法允许指定预期的消息、参数和调用计数。在下面的例子中，`Random`的一个实例预计会收到带有类`Range`参数的`rand`消息 6 次。

```
expect_any_instance_of(Random).to receive(:rand).with(instance_of(Range))
                                                .exactly(6).times
random = Random.new
6.times { random.rand(1..10) }
```

使用“`expect_any_instance_of`”模拟多个类实例将产生类似“消息‘rand’已被# < Random:1500 >接收，但已被#<Random:0x 00007 f 94 f 01 c8 EC 8>接收”的错误消息。为了模仿多个实例，可以结合使用类级别的部分 double 和实例级别的验证 double，如用例章节所述。

```
# This will NOT work!
# expect_any_instance_of(Random).to receive(:rand).with(instance_of(Range))
#                                                .exactly(6).times
# 6.times { Random.new.rand(1..10) }
```

值得注意的是，使用部分 double 对类实例的方法进行存根化或嘲讽不会调用原始代码。其他没有被模仿的方法将调用原始代码。请参见下面的示例。

```
expect_any_instance_of(Array).to receive(:append).with(instance_of(Integer)).once
arr = []
arr.append(1)
arr << 2
expect(arr).to eq [2]
```

为了调用存根或模仿方法的原始代码，请使用`and_call_original.`

```
expect_any_instance_of(Array).to receive(:append).with(instance_of(Integer))
                                                 .once
                                                 .and_call_original
arr = []
arr.append(1)
arr << 2
expect(arr).to eq [1, 2]
```

下面的例子展示了如何删除一个类。

```
allow(Random).to receive(:rand).and_return(7)
expect(Random.rand).to eq(7)
```

下面的例子展示了如何模仿一个类。

```
expect(Random).to receive(:rand).exactly(6).times
6.times { Random.rand }
```

### 匹配参数

可以使用 with 方法约束模拟以匹配特定的参数。此方法接受一个参数列表，该列表应与存根或模仿对象在消息中接收的参数列表相匹配。

```
expect_any_instance_of(Array).to receive(:append).with(1, 2, 'c', true)
Array.new.append(1, 2, 'c', true)
```

也可以使用 RSpec 匹配器，根据特定的匹配器允许不同的参数变量。

```
expect_any_instance_of(Array).to receive(:append).with(
                                                        instance_of(Integer), 
                                                        kind_of(Numeric), 
                                                        /c+/, 
                                                        boolean
                                                      ).twice.and_call_original
Array.new.append(1, 2, 'c', true)
         .append(2, 3.0, 'cc', false)
```

在上面的例子中，应用了 4 个不同的匹配器:

*   `instance_of` -将匹配指定类的任何实例，例如 instance_of(Integer)将匹配任何整数，但不匹配任何浮点数
*   `kind_of` -将匹配其祖先列表中具有指定类或模块的任何实例，例如 kind_of(Numeric)将匹配任何扩展 Numeric 的数字，包括整数和浮点数
*   `Regexp instance` -将匹配任何匹配指定正则表达式的字符串
*   `boolean` -将匹配任何布尔值，即真或假

也可以使用对应于任何参数的匹配器。以下示例将匹配任何参数，只要有 4 个参数。

```
expect_any_instance_of(Array).to receive(:append).with(
                                                        anything, 
                                                        anything,
                                                        anything, 
                                                        anything
                                                      ).twice.and_call_original
Array.new.append(1, 2, 'c', true)
          .append(2, 3.0, 'cc', false) 
```

使用`any_args`匹配任意长(或空)的参数列表。下面的例子将匹配任何参数，只要最后一个参数是`true`。

```
expect_any_instance_of(Array).to receive(:append).with(any_args, true)
                                                 .thrice.and_call_original
Array.new.append(1, 2, 'c', true)
          .append(1, 2, 3, {}, [], true)
          .append(true)
```

可以使用 hash_including matcher 来匹配散列。下面的例子将匹配任何包含关键字`:nationality`、:`addresss`、`:name`的散列。

```
expect_any_instance_of(Array).to receive(:append)
                                  .with(hash_including(
                                                        :nationality, 
                                                        :address, 
                                                        :name
                                                      ))
[].append({ name: 'Alice', address: {}, nationality: 'American', 
            occupation: 'engineer' }) 
```

可以期待键以及键值对。两种变体都可以混合使用，只要键值对位于`hash_including`的参数列表的末尾，如下例所示。

```
expect_any_instance_of(Array).to receive(:append)
                                    .with(
                                      hash_including(
                                        :nationality, 
                                        :address, 
                                        name: 'Alice'
                                      ))
[].append({ name: 'Alice', address: { city: 'Miami', street: 'NW 12th Ave' }, 
            nationality: 'American' })
```

允许嵌套`hash_including`匹配器。请参见下面的示例。

```
expect_any_instance_of(Array).to receive(:append)
                                    .with(
                                      hash_including(
                                        :nationality,
                                        address: hash_including(:city, :street),
                                        name: 'Alice'
                                      ))
[].append({ name: 'Alice', address: { city: 'Miami', street: 'NW 12th Ave' }, 
            nationality: 'American' })
```

为了匹配数组`array_including`，可以应用 matcher，方式类似于`hash_including`。

在大多数情况下，不同的匹配器可以混合使用，例如，`hash_including`可以使用`instance_of`作为它的一些元素的值的匹配器。RSpec 匹配器的概述可以在[这里](http://web.archive.org/web/20220924171519/https://relishapp.com/rspec/rspec-mocks/v/3-2/docs/setting-constraints/matching-arguments)找到。

### 看得更远

所展示的例子是基本的构建模块，在一些约束条件下，它们可以混合起来产生最终的测试实现。例如:

*   存根功能(应该返回什么)可以与模仿功能(关于消息的期望是什么)混合。

```
expect_any_instance_of(Random).to receive(:rand)
                                    .with(instance_of(Range))
                                    .exactly(6).times
                                    .and_return(1,1,2,3,5,8)
```

```
random_double = instance_double(Random)
allow(random_double).to receive(:rand).and_return(1,1,2,3,5,8)
allow(Random).to receive(:new).and_return(random_double)
```

更复杂的用例将在下一节介绍。

## 用例:简单的轮询应用程序

本节介绍的用例实现了一个简单轮询应用程序的测试。

### 应用概述

该应用程序纯粹是为教育目的而构建的，由于其简单性，没有真正的用途。实施细节可在附录中找到。

该应用程序包括 3 个类别:

*   反馈代表一种形式，允许用户使用喜欢和不喜欢的动作来评估主题。可以使用微移来微移反馈！方法。[轻推](http://web.archive.org/web/20220924171519/https://www.goodreads.com/en/book/show/3450744-nudge)是指投票创建者试图通过修改问题等方式影响用户评估的过程。
*   参与者代表参与投票的用户。用户使用反馈表来评估给定的主题。
*   Poll 代表一个轮询过程。它将被评估的参与者和对象的名字作为输入。基于这些，它创建参与者和反馈。它可以通过要求每个参与者使用反馈表来评估每个主题来进行投票。民意调查推动了其中一个反馈，目的是扭曲评估结果。投票结束后，它会以主题排名的形式产生投票结果，并根据喜欢的数量进行排序。

测试代码无法访问轮询过程的内部，但是它可以使用 RSpec doubles 来了解轮询过程是如何进行的。

### 测试设置

默认情况下，测试使用下面的初始化代码。

```
RSpec.describe Poll do
  let(:names) { %w(alice adam peter kate) }
  let(:subjects) { %w(math physics history biology) }
  subject { described_class.new(names: names, subjects: subjects) }
```

一个 RSpec `subject`包含对一个`Poll`实例的引用，该实例已经接收了 4 个参与者`names`的列表和 4 个待评估的`subjects`的列表。这里的 RSpec `subject`属于 RSpec 测试环境的领域，不应该与属于轮询应用领域的`subjects`混淆。

RSpec `subject`表示测试的主题，将由单独的测试使用，而`subjects`将被传递给`feedbacks`，后者将由轮询应用程序中的`participants`进行评估。

必要时，输入`names`和`subjects`可以通过覆盖单个测试中的`let`定义来修改，例如:

```
context 'when 2 participant and 4 subjects' do
  let(:names) { %w(alice adam) }

  it 'instantiates 2 participants' do
    # TEST IMPLEMENTATION
  end
end
```

### 场景:测试一个类已经被实例化

假设我们需要测试参与者在轮询实例化时是否用正确的参数实例化。

#### 变式 1。实例不接收消息

我们可以用偏双- `expect(Participant)`。`with`方法验证是否传递了正确的参数。每组参数应该发送一次。从测试代码的角度来看，对象实例化的顺序并不重要。

```
context 'when 2 participant and 4 subjects' do
  let(:names) { %w(alice adam) }

  it 'instantiates 2 participants' do
    expect(Participant).to receive(:new).with(hash_including(name: 'adam')).once
    expect(Participant).to receive(:new).with(hash_including(name: 'alice')).once
    subject
  end
end
```

#### 变式 2。实例接收消息

在之前的变体中，向新实例化的`Participant`对象发送消息会导致错误，因为在部分 double 上调用`new`方法会返回`nil.`，我们可以通过应用`and_call_original`来解决这个问题，这将导致部分 double 返回相同的值，就好像该方法是在原始类上调用的一样，即有效的`Participant`对象。

```
context 'when 2 participant and 4 subjects' do
  let(:names) { %w(alice adam) }

  it 'instantiates 2 participants' do
    expect(Participant).to receive(:new).with(hash_including(name: 'adam'))
                                        .and_call_original.once
    expect(Participant).to receive(:new).with(hash_including(name: 'alice'))
                                        .and_call_original.once
    subject.run
  end
end
```

### 场景:测试类的实例

假设我们需要测试`Participant`的一个实例接收评估请求。

#### 变式 1。一个例子

在消息将只发送到`Participat`的一个实例的情况下，我们可以使用`expect_any_instance_of`部分 double。与前面的例子类似，`with`用于验证方法参数，`exactly(n).times`用于指定收到的`evaluate`消息的数量。

```
context 'when 1 participant and 4 subjects' do
  let(:names) { %w(alice) }

  it 'should ask participant for evaluation 4 times' do
    expect_any_instance_of(Participant).to receive(:evaluate)
                                  .with(instance_of(Feedback)).exactly(4).times
    subject.run
  end
end
```

#### 变式 2。多个实例

在消息将被发送到多个`Participant`实例的情况下，`expect_any_instance_of`部分双精度将失败，并显示如下消息:`The message 'evaluate' was received by #<Participant:1720 @name=adam> but has already been received by #<Participant:0x00007fea7f091368>`

为了解决这个问题，我们可以使用一个验证 double `instance_double(Participant)`并允许它接收`evaluate`消息。此外，我们应该存根`Participant`类返回验证双响应`new`消息。每个新实例化的`Participant`实际上将是同一个对象，即验证双对象。然后，我们可以注册对任意数量的`Participant`实例的调用。

```
context 'when 4 participants and 4 subjects' do
  it 'should ask participants for evaluation 16 times' do
    fake_participant = instance_double(Participant)
    allow(fake_participant).to receive(:evaluate)
    allow(Participant).to receive(:new).and_return(fake_participant)

    expect(fake_participant).to receive(:evaluate)
                                  .with(instance_of(Feedback)).exactly(16).times
    subject.run
  end
end
```

### 场景:在一个实例中引发错误

假设当许多`Participant`实例中的一个评估失败时，我们想要测试`Poll`的行为。我们可以使用偏双`allow_any_instance_of`结合`and_wrap_original`的方法，如下图所示。

传递给`and_wrap_original`的块接收代表`evaluate`方法的`Method`对象作为第一个参数。剩余的参数对应于最初传递给`evaluate`方法的参数。在程序块内部，我们可以通过检查`evaluate`方法的接收者，例如检查参与者的名字，来识别一个特定的`Participant`对象，在这个对象上`evaluate`被调用。

在下面的代码片段中，当一个名为`Adam`的参与者收到`math`进行评估时，我们发送一个错误(`false`)。否则，调用接收原始参数的原始`evaluate`方法。

```
context 'when 1 participant fails to evaluate 1 subject' do
  context 'when 4 participants and 4 subjects' do
    it 'produces 1 error' do
      allow_any_instance_of(Participant).to receive(:evaluate)
                                              .and_wrap_original do |evaluate_method, feedback|
        is_adam = evaluate_method.receiver.name == 'adam'
        is_math = feedback.subject == 'math'
        (is_adam && is_math) ? false : evaluate_method.call(feedback)
      end
      expect { subject.run }.to change { subject.error_count }.from(0).to(1)
                            .and change{ subject.vote_count }.from(0).to(15)
    end
  end
end
```

### 场景:预先不知道预期参数的测试实例

假设我们需要测试一次,`Feedback`的实例收到了带有特定参数的`nudge!`消息，但是在轮询结束之前，我们无法知道参数是什么。正如我们在前面的例子中所做的那样，我们不能期望在投票之前会有什么样的争论。

#### 变式 1。使用间谍

为了实现这个目标，我们可以使用一个验证间谍`instance_spy(Feedback)`来跟踪所有发送的消息。此外，我们将使用部分 double `Feedback`并将其存根返回验证间谍，以响应新消息。每个新实例化的`Feedback`实际上将是同一个对象，即验证间谍。因为 spy 登记接收到的消息和相应的参数，所以我们将能够在轮询结束后检查它。

```
context 'when 4 participants and 4 subjects' do
  it 'nudges one feedback' do
    fake_feedback = instance_spy(Feedback)
    allow(Feedback).to receive(:new).and_return(fake_feedback)
    nudge_template = subject.run
    expect(fake_feedback).to have_received(:nudge!).with(nudge_template).once
  end
end
```

#### 变式 2。使用双精度

除了一个例外，使用验证 double `instance_double(Feedback)`可以实现相同的目标。我们必须明确定义允许双精度浮点数接收的消息。就结果而言，两种变体是等效的，但是就测试设置而言，验证 double 需要更多的工作。

```
context 'when 4 participants and 4 subjects' do
  it 'nudges one feedback' do
    fake_feedback = instance_double(Feedback)
    allow(fake_feedback).to receive(:nudge!)
    allow(fake_feedback).to receive(:nudged?)
    allow(fake_feedback).to receive(:like)
    allow(fake_feedback).to receive(:dislike)
    allow(Feedback).to receive(:new).and_return(fake_feedback)
    nudge_template = subject.run
    expect(fake_feedback).to have_received(:nudge!).with(nudge_template).once
  end
end
```

## 摘要

介绍了 RSpec 模拟的以下方面:

*   嘲讽技巧的优点、来源和类型。
*   RSpec 中 mocking 的应用及其对纯 doubles、验证 doubles 和部分 doubles 的划分，以及 doubles 的匹配论元。
*   对于如何使用 RSpec 构建测试的总结，对于典型用例的选择也是如此。

RSpec mocking 所讨论的方面将有助于您提高单元测试技能，并加强您对 [Ruby 代码](/web/20220924171519/https://www.netguru.com/blog/ruby-versus-ruby-on-rails)的测试驱动开发。

## 附录

应用程序由 4 个文件组成。它要求安装 RSpec 3。可以使用 `rspec poll_spec.rb`运行测试

### 反馈. rb

```
class Feedback
  attr_reader :subject, :likes, :dislikes

  def initialize(**args)
    @subject = args[:subject] || 'default'
    @likes, @dislikes = 0, 0
    @nudge = nil
  end

  def like
    @likes += 1
  end

  def dislike
    @dislikes += 1
  end

  def nudge!(data)
    @nudge = data
  end

  def nudged?
    @nudge
  end
end
```

### participant.rb

```
class Participant
  attr_reader :name

  def initialize(**args)
    @name = args[:name] || 'anonymous'
  end

  def evaluate(feedback)
    like_factor = feedback.nudged? ? 10 : 1
    ((rand 0..like_factor) > 0) ? feedback.like : feedback.dislike
  end
end
```

### poll.rb

```
require './feedback.rb'
require './participant.rb'

class Poll
  attr_reader :vote_count, :error_count

  def initialize(**args)
    @feedbacks = args[:subjects].map{|subject| Feedback.new(subject: subject) }
    @participants = args[:names].map{|name| Participant.new(name: name)}
    @error_count = 0
  end

  def run
    @feedbacks.sample.nudge!(nudge_template)
    @feedbacks.each do |feedback|
      @participants.each do |participant|
        @error_count += 1 unless participant.evaluate(feedback)
      end
    end
    nudge_template
  end

  def vote_count
    @feedbacks.inject(0) do |memo, feedback|
      memo += (feedback.likes + feedback.dislikes)
    end
  end

  def ranking
    @feedbacks.sort{|a,b| b.likes <=> a.likes }
  end

  private

  def nudge_template
    @nudge_template ||= ('nudge template ' << rand(1..10).to_s)
  end
end
```

### poll_spec.rb

```
require './poll.rb'

RSpec.describe Poll do
  let(:names) { %w(alice adam peter kate) }
  let(:subjects) { %w(math physics history biology) }
  subject { described_class.new(names: names, subjects: subjects) }

  describe 'test a class has been instantiated' do
    context 'instances do NOT receive messages' do
      context 'when 2 participant and 4 subjects' do
        let(:names) { %w(alice adam) }

        it 'instantiates 2 participants' do
          expect(Participant).to receive(:new).with(hash_including(name: 'adam')).once
          expect(Participant).to receive(:new).with(hash_including(name: 'alice')).once
          subject
        end
      end
    end

    context 'instances receive messages' do
      context 'when 2 participant and 4 subjects' do
        let(:names) { %w(alice adam) }

        it 'instantiates 2 participants' do
          expect(Participant).to receive(:new).with(hash_including(name: 'adam'))
                                              .and_call_original.once
          expect(Participant).to receive(:new).with(hash_including(name: 'alice'))
                                              .and_call_original.once
          subject.run
        end
      end
    end
  end

  describe 'test instances of a class' do
    context 'single instances' do
      context 'when 1 participant and 4 subjects' do
        let(:names) { %w(alice) }

        it 'asks participant for evaluation 4 times' do
          expect_any_instance_of(Participant).to receive(:evaluate)
                                                   .with(instance_of(Feedback)).exactly(4).times
          subject.run
        end
      end
    end

    context 'multiple instances' do
      context 'when 4 participants and 4 subjects' do
        it 'asks participants for evaluation 16 times' do
          fake_participant = instance_double(Participant)
          allow(fake_participant).to receive(:evaluate)
          allow(Participant).to receive(:new).and_return(fake_participant)

          expect(fake_participant).to receive(:evaluate)
            .with(instance_of(Feedback)).exactly(16).times
          subject.run
        end
      end
    end
  end

  describe 'induce error in one of the instances' do
    context 'when 1 participant fails to evaluate 1 subject' do
      context 'when 4 participants and 4 subjects' do
        it 'produces 1 error' do
          allow_any_instance_of(Participant).to receive(:evaluate)
                                                  .and_wrap_original do |evaluate_method, feedback|
            is_adam = evaluate_method.receiver.name == 'adam'
            is_math = feedback.subject == 'math'
            (is_adam && is_math) ? false : evaluate_method.call(feedback)
          end
          expect { subject.run }.to change { subject.error_count }.from(0).to(1)
                                .and change{ subject.vote_count }.from(0).to(15)
        end
      end
    end
  end

  describe 'test instances with expected arguments not known in advance' do
    context 'using spy' do
      context 'when 4 participants and 4 subjects' do
        it 'nudges one feedback' do
          fake_feedback = instance_spy(Feedback)
          allow(Feedback).to receive(:new).and_return(fake_feedback)
          nudge_template = subject.run
          expect(fake_feedback).to have_received(:nudge!).with(nudge_template).once
        end
      end
    end

    context 'using double' do
      context 'when 4 participants and 4 subjects' do
        it 'nudges one feedback' do
          fake_feedback = instance_double(Feedback)
          allow(fake_feedback).to receive(:nudge!)
          allow(fake_feedback).to receive(:nudged?)
          allow(fake_feedback).to receive(:like)
          allow(fake_feedback).to receive(:dislike)
          allow(Feedback).to receive(:new).and_return(fake_feedback)
          nudge_template = subject.run
          expect(fake_feedback).to have_received(:nudge!).with(nudge_template).once
        end
      end
    end
  end
end
```