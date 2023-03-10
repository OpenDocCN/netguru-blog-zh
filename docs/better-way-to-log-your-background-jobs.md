# 记录后台工作的更好方法

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/better-way-to-log-your-background-jobs>

 让我们面对现实:没有后台工作，在 Rails 中构建任何重要的应用程序都是不可能的。有几个用例，其中一些可能是:周期性任务、太重而不能以同步方式返回结果的任务或者可以在流程之外的任务，它们的失败不会对当前处理的流程产生任何影响。

对我们开发者来说幸运的是，因为后台处理在 Rails 中被广泛使用，我们有一些很棒的工具和宝石来帮助我们！最受欢迎的是 [Sidekiq](http://web.archive.org/web/20220925011812/https://infinum.co/the-capsized-eight/analyzing-rubygems-stats-v2017) ，这是我将在代码<g class="gr_ gr_88 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="88" data-gr-id="88">中使用的</g>。

太好了，现在几乎每个人都在使用后台处理，但是这个 *<g class="gr_ gr_87 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del" id="87" data-gr-id="87">记录</g>* 的东西是怎么回事呢？Sidekiq 进程不是默认生成日志并保存到文本文件吗？是的，但是这些日志很难看，很难阅读，很难汇总，几乎不可能自动做出决定(除非你正在使用类似[麋鹿栈](http://web.archive.org/web/20220925011812/https://www.elastic.co/elk-stack)的东西)。这篇博文将给出一个例子，说明如何只需做一点工作就可以记录异步作业，以及如何将隐藏的日志用于各种目的。

## **步骤 1 -创建日志表**

是的，我们将把日志保存到数据库中。结构可能取决于您的需求。我想确定的是:作业类型、作业的<g class="gr_ gr_100 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="100" data-gr-id="100">当前</g> `state` <g class="gr_ gr_100 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="100" data-gr-id="100">、</g> `jid`(作业 ID)、关联的用户 id (可选)和`created_at` / `updated_at`时间戳。让我们也在<g class="gr_ gr_101 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="101" data-gr-id="101">`jid`<g class="gr_ gr_101 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="101" data-gr-id="101">列</g>上添加一个索引。</g>

```
class CreateAsyncJobLogs < ActiveRecord::Migration[5.0]
  def change
    create_table :async_job_logs do |t|
      t.string  :jid
      t.integer :state
      t.integer :job_type
      t.references :user, foreign_key: true

      t.timestamps
    end
  end

  add_index(:async_job_logs, :jid)
end 
```

## **步骤 2 -创建模型**

现在，让我们快速的<g class="gr_ gr_95 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="95" data-gr-id="95">添加</g> `AsyncJobLog` <g class="gr_ gr_95 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="95" data-gr-id="95"><g class="gr_ gr_94 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar only-ins doubleReplace replaceWithoutSep" id="94" data-gr-id="94">型号</g></g> 带枚举。你可能想要添加更多的状态，比如*失败*或者*死亡*。为了简单起见，我将只使用*开始*和*结束*。作业的类型同样取决于后台作业所负责的内容。

```
class AsyncJobLog < ApplicationRecord
  enum state: {
    started:  1,
    finished: 2,
  }

  enum job_type: {
    reconciliation:          1,
    user_verification:       2,
    financial_data_import:   3,
  }
end 
```

## **步骤 3 -为您的员工添加日志记录**

让我们从负责大部分脏活的模块开始——也就是创建和更新日志:

```
module WithJobLogging
  def log_retryable_job(jid, job_type, user_id = nil)
    create_async_job_log(jid, job_type, user_id) unless async_job_log(jid)
    yield
    mark_job_as_finished(jid)
  end

  def create_async_job_log(jid, job_type, user_id = nil)
    AsyncJobLog.create(
      jid: jid,
      state: 'started',
      job_type: job_type,
      user_id: user_id
    )
  end

  def async_job_log(jid)
    AsyncJobLog.find_by(jid: jid)
  end

  def mark_job_as_finished(jid)
    async_job_log(jid).finished!
  end
end 
```

现在让我们在实际工作者中使用这个模块:

```
require 'sidekiq'

class FinancialDataImportWorker
  include Sidekiq::Worker
  include WithJobLogging
  sidekiq_options retry: 5

  def perform(user_id)
    log_retryable_job(jid, 'financial_data_import', user_id) { do_stuff }
  end

  private

  def do_stuff
    # logic
  end
end 
```

流程非常简单——在工人完成工作之前，它首先在表中创建一条记录，提供一些关于他是谁的信息。此时，我们有一条记录，说明负责财务数据导入(`job_type`)并与某个用户(`user_id`)相关联的工作人员已经在给定时间(`created_at`)开始处理了——这已经是很多很好的聚合信息了。在处理完工人负责的任何事情后，它只需找到自己的日志(由 JID 完成)并将其标记为已完成。但是，如果我的工作线程多次失败，直到达到最大重试次数，该怎么办呢？嗯，我们可以用很多方法来处理这个问题:这完全取决于你的需求和惯例是什么。让我们来看一个最简单的例子，更新我们的 worker，以便它在最后一次重试时将日志标记为完成:

```
require 'sidekiq'

class FinancialDataImportWorker
  include Sidekiq::Worker
  include WithJobLogging
  sidekiq_options retry: 5

  sidekiq_retries_exhausted do |msg|
    async_job_log = AsyncJobLog.find_by(jid: msg['jid'])
    async_job_log.finished!
  end

  def perform(user_id)
    log_retryable_job(jid, 'financial_data_import', user_id) { do_stuff }
  end

  private

  def do_stuff
    # logic
  end
end 
```

如果您的应用程序可以利用关于作业不成功的信息，您可能希望用新的状态更新作业日志，比如 *dead* (不要忘记将它添加到您的枚举散列中)。当我们这样做的时候——那些根本不允许重试的作业怎么办？这是解决这个问题的另一种方法。为了简单起见，让我们将<g class="gr_ gr_155 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar only-ins doubleReplace replaceWithoutSep" id="155" data-gr-id="155">新的</g>方法添加到我们的模块中，但是在现实世界中，您可能想要创建一个单独的模块，并使用<g class="gr_ gr_156 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar only-ins doubleReplace replaceWithoutSep" id="156" data-gr-id="156">描述性的</g>名称:

```
module WithJobLogging
  def log_retryable_job(jid, job_type, user_id = nil)
    create_async_job_log(jid, job_type, user_id) unless async_job_log(jid)
    yield
    mark_job_as_finished(jid)
  end

  def log_nonretryable_job(jid, job_type, user_id = nil)
    begin
      create_async_job_log(jid, job_type, user_id) unless async_job_log(jid)
      yield
    rescue
      # if needed
    ensure
      mark_job_as_finished(jid)
    end
  end

  def create_async_job_log(jid, job_type, user_id = nil)
    AsyncJobLog.create(
      jid: jid,
      state: 'started',
      job_type: job_type,
      user_id: user_id
    )
  end

  def async_job_log(jid)
    AsyncJobLog.find_by(jid: jid)
  end

  def mark_job_as_finished(jid)
    async_job_log(jid).finished!
  end
end 
```

现在我们可以在<g class="gr_ gr_109 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="109" data-gr-id="109">非</g> - <g class="gr_ gr_110 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="110" data-gr-id="110">可重试</g>工作器中使用它，就像其他方法一样:

```
require 'sidekiq'

class UserVerificationWorker
  include Sidekiq::Worker
  include WithJobLogging
  sidekiq_options retry: false

  def perform(user_id)
    log_nonretryable_job(jid, 'user_verification', user_id) { do_stuff }
  end

  private

  def do_stuff
    # logic
  end
end 
```

我们只是确保无论在 worker 的主程序块中发生什么，我们都会以标记为 *finished* 的日志结束。如果您想根据收集的信息做出决策，在日志中反映正确的作业状态是至关重要的。

## **步骤 4 -使用收集的信息**

好的，我们把日志很好的组织起来并保存在数据库里。现在怎么办？你可以用日志做很多事情。现在您知道了哪个工人在什么时间为哪个用户执行了操作。您可以向这些日志添加各种类型的字段，选项是无限的(实际上受到最大列数的限制)，例如计算的处理时间、<g class="gr_ gr_92 gr-alert gr_gramm gr_inline_cards gr_run_anim Punctuation only-del replaceWithoutSep" id="92" data-gr-id="92">重试次数、</g>对其他相关模型的引用等等。

您可能会想到手动处理这些日志，以便在 Active Admin 中使用它们进行调试或显示/过滤。让我们来看一个可能的场景，代码使用这些作为逻辑的一部分。

### **临界区**

我们之前定义了几种作业类型。让我们假设不可能有多个与财务相关的工作人员同时为同一个用户运行。这意味着协调工作器和财务数据导入工作器不能同时运行。首先，让我们给`AsyncJobLog`模型添加一个方便的范围:

```
class AsyncJobLog < ApplicationRecord
  enum state: {
    started:  1,
    finished: 2,
  }

  enum job_type: {
    reconciliation:          1,
    user_verification:       2,
    financial_data_import:   3,
  }

  scope :financial_jobs, -> { where(job_type: %w[financial_data_import reconciliation]) }
end 
```

接下来，在<g class="gr_ gr_132 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="132" data-gr-id="132">我们的</g>`FinancialDataImportWorker`<g class="gr_ gr_132 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="132" data-gr-id="132">中，让我们检查一下是否可以在之前进行<g class="gr_ gr_131 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="131" data-gr-id="131">甚至创建一个作业日志:</g></g>

```
require 'sidekiq'

class FinancialDataImportWorker
  include Sidekiq::Worker
  include WithJobLogging
  sidekiq_options retry: 5

  sidekiq_retries_exhausted do |msg|
    async_job_log = AsyncJobLog.find_by(jid: msg['jid'])
    async_job_log.finished!
  end

  def perform(user_id)
    user = User.find(user_id)
    return if financial_job_in_progress?(user)

    log_retryable_job(jid, 'financial_data_import', user_id) { do_stuff }
  end

  private

  def financial_job_in_progress?(user)
    user.async_job_logs.financial_jobs.started.where.not(jid: jid).any?
  end  

  def do_stuff
    # logic
  end
 end 
```

如果该用户的两个财务工作者中有一个已经在进行中，那么该工作者将简单地退出而不开始。当然，辞职是最简单的选择。您可以引发 Rollbar 错误或做任何对您的应用程序有意义的事情。为了让它成为真正的临界区，你可以让工人在一定的时间间隔内重试<g class="gr_ gr_128 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="128" data-gr-id="128">(别忘了设置某种 TTL！).你可能想知道<g class="gr_ gr_134 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="134" data-gr-id="134">从何而来</g> `where.not(jid: jid)` <g class="gr_ gr_134 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="134" data-gr-id="134">从何而来</g>。如果您有 <g class="gr_ gr_135 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar only-ins doubleReplace replaceWithoutSep" id="135" data-gr-id="135"><g class="gr_ gr_108 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="108" data-gr-id="108">可重试的</g></g> 工作线程，当日志失败时，它不会将日志标记为已完成，那么您基本上让关键部分对任何与财务相关的作业关闭。这很好，但是你需要某种“后门”让失败的工人重试——这就是你的后门。Sidekiq 作业即使在重试时也仍然使用相同的唯一 JID。这意味着我们可以使用这个 JID 来授权重试工作者进入临界区并尝试完成作业。</g>

*PS。如果临界区非常关键，可能值得在数据库中创建唯一的 T2 键，以确保您不会得到任何脏读/写，甚至在非常不幸的事件中也不会让两个金融工作者同时产生。*

## **结论**

我希望这能让您了解如何轻松设置后台作业日志记录，以及使用简单日志记录机制的优势。异步处理绝对不容易，这方面的调试问题也不容易。日志工作器<g class="gr_ gr_114 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="114" data-gr-id="114">给了</g>更多关于幕后发生的事情的信息，也可以在代码中用来调整流程——无论是在同步还是异步的情况下。一旦你设置好了，你可能会想出如何使用日志记录的其他方法——总之，每个应用程序都可能需要关于后台作业的非常具体的信息。也要注意这不是<g class="gr_ gr_116 gr-alert gr_gramm gr_hide gr_inline_cards gr_run_anim Grammar only-ins multiReplace replaceWithoutSep replaceWithoutSep" id="116" data-gr-id="116">一刀切</g>的解决方案。如果您需要更好的性能，您可能需要调整它，例如使用 JID 创建的 UUID 或使用 [sidekiq-lock](http://web.archive.org/web/20220925011812/https://github.com/emq/sidekiq-lock) 或[Sidekiq-unique-jobs](http://web.archive.org/web/20220925011812/https://github.com/mhenrixon/sidekiq-unique-jobs)中支持的 Sidekiq 锁定机制 。为高负载系统选择解决方案时要谨慎，并确保它们经过充分测试！ 祝好运，玩得开心！

* * *

非常感谢[马切伊<g class="gr_ gr_97 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="97" data-gr-id="97">曼斯菲尔德</g>帮助我们充分利用这篇文章！](http://web.archive.org/web/20220925011812/https://github.com/mensfeld)

照片由 [马切伊鲁塞克](http://web.archive.org/web/20220925011812/https://unsplash.com/photos/XiQQQ0R7vfI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [下](http://web.archive.org/web/20220925011812/https://unsplash.com/@robone?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)