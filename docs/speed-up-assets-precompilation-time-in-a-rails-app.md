# 如何在 Ruby on Rails 中加快资产预编译时间

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/speed-up-assets-precompilation-time-in-a-rails-app>

 每个 Rails 开发人员都知道加载资产非常耗时。这里有一些关于如何加快速度的有用提示。

## 技巧

*   不要将所有内容打包成一个大文件。将 CDN 用于 CKeditor 等工具，并且只在需要它的页面上需要它。
*   将翻译保存在单独的文件中。仅配置所需的语言。O <g class="gr_ gr_39 gr-alert gr_gramm gr_inline_cards gr_run_anim Punctuation only-ins replaceWithoutSep" id="39" data-gr-id="39">否则，</g>翻译文件可能会随着每个新的库一起膨胀得非常快。如果您只配置所需的语言，您将避免大的编译文件，这意味着更快的编译和页面加载。

    ```
    config.i18n.available_locales = %i(en)
    ```

*   <g class="gr_ gr_47 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="47" data-gr-id="47">去掉</g>`gem 'therubyracer'`<g class="gr_ gr_48 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="48" data-gr-id="48"><g class="gr_ gr_47 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="47" data-gr-id="47">从</g></g>`Gemfile`<g class="gr_ gr_48 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="48" data-gr-id="48">因为</g>这个宝石使用了大量的内存。请确保安装了最新版本的节点。 [https://dev center . heroku . com/articles/rails-asset-pipeline # therubyracer](http://web.archive.org/web/20220929114734/https://devcenter.heroku.com/articles/rails-asset-pipeline#therubyracer)
*   请勿使用`require``require_tree`<g class="gr_ gr_51 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="51" data-gr-id="51"><g class="gr_ gr_50 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="50" data-gr-id="50">和</g></g>`require_self`<g class="gr_ gr_51 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="51" data-gr-id="51">在</g> your SASS/SCSS 文件中。它们非常原始，不能很好地处理 Sass 文件。相反，使用 Sass 的<g class="gr_ gr_52 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="52" data-gr-id="52">原生</g>`@import`<g class="gr_ gr_52 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="52" data-gr-id="52">指令，</g>Sass-rails 已经定制了这些指令来与您的 Rails 项目的约定相集成。 [https://github.com/rails/sass-rails#important-note](http://web.archive.org/web/20220929114734/https://github.com/rails/sass-rails#important-note) 
*   在你的 SASS/SCSS 文件<g class="gr_ gr_45 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="45" data-gr-id="45">中小心使用</g>`@import`<g class="gr_ gr_45 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="45" data-gr-id="45">指令</g>。避免将所有的包资产<g class="gr_ gr_53 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="53" data-gr-id="53">导入为</g>`@import 'compass';`<g class="gr_ gr_53 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="53" data-gr-id="53">时只能<g class="gr_ gr_54 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="54" data-gr-id="54">做</g></g>`@import 'compass/css3/flexbox'`<g class="gr_ gr_54 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="54" data-gr-id="54">。</g>
*   避免<g class="gr_ gr_35 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="35" data-gr-id="35">使用</g>`require_tree .`<g class="gr_ gr_35 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="35" data-gr-id="35"><g class="gr_ gr_34 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar only-ins doubleReplace replaceWithoutSep" id="34" data-gr-id="34">指令</g></g> 在你的 JS/COFFEE 中体现。让我们想象一个<g class="gr_ gr_36 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar only-ins doubleReplace replaceWithoutSep" id="36" data-gr-id="36">场景</g>，其中你的应用有一个管理面板，有单独的资产: 

    ```
    //** assets/javascripts/admin/admin.js  //= require admin/tab.js //= ...
    ```

    ```
    //** assets/javascripts/application.js  //= require 'something' //= require_tree . // BAD! it requires admin assets as well
    ```

*   编译资产时检查日志。通过访问调试模式，可以很容易地覆盖默认的日志记录器，并查看幕后发生了什么。

    ```
    # /lib/tasks/assets.rake  require 'sprockets/rails/task'  Sprockets::Rails::Task.new(Rails.application) do |t|   t.logger = Logger.new(STDOUT) end
    ```

## 汇总

以上提示可以帮助你更快的检索到你编译好的资产！作为概念的证明，请查看一下[<g class="gr_ gr_32 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="32" data-gr-id="32">cintrzyk</g>/链轮-提示](http://web.archive.org/web/20220929114734/https://github.com/cintrzyk/sprockets-tips)，在这里你可以[找到一个带有更多细节的示例 Rails 应用](/web/20220929114734/https://www.netguru.com/services/ruby-on-rails-development)。Rails 5.1 引入了其他一些管理资产的方式。我强烈推荐你<g class="gr_ gr_204 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="204" data-gr-id="204">用</g>纱搭配 Webpack，<g class="gr_ gr_205 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling multiReplace" id="205" data-gr-id="205">值得</g>相信我！您所能获得的是资产依赖的管理、更高效的编译过程、无需页面刷新的代码热重装、ES6 支持、PostCSS 等等！