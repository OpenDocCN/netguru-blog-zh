# TIL #2:如何在你的 Rails 应用程序 URL 中包含语言代码

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/til-2-how-to-include-language-code-in-rails-app>

 在今天下午...

Ruby on Rails！

当您开发多语言应用程序时，有时您可能会遇到在 URL 中包含语言代码的需求。

The same happened to me and I found one extremely simple trick that will allow you to:

1.在你的 URL 中总是要有语言代码。

2.当有人试图在没有给出语言代码的情况下访问你的应用时，将用户重定向到正确的 URL。

你需要做的只是稍微改变一下你的 config/routes.rb，开始吧:

很简单，不是吗？

```
 # config/routes.rb

MyApp::Application.routes.draw do
  scope "/:locale", locale: /#{I18n.available_locales.join("|")}/ do
    # your routes go here

    root to: "home#index"
  end

  root to: redirect("/#{I18n.default_locale}", status: 302), as: :redirected_root
  get "/*path", to: redirect("/#{I18n.default_locale}/%{path}", status: 302),
                constraints: { path: /(?!(#{I18n.available_locales.join("|")})\/).*/ },
                format: false
end 
```

今天我了解到，TIL 是我们的开发者分享他们每天发现的最好的技术的地方。你可以为一些问题找到聪明的解决方案，有用的  <g class="gr_ gr_171 gr-alert gr_spell gr_inline_cards gr_disable_anim_appear ContextualSpelling ins-del multiReplace" id="171" data-gr-id="171"><g class="gr_ gr_178 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar multiReplace" id="178" data-gr-id="178">建议</g></g> 以及任何可以让你的开发者生活更轻松的东西。

照片由 [萨拉里亚诺](http://web.archive.org/web/20221002001519/https://unsplash.com/photos/qoKTJo2zpRs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [下](http://web.archive.org/web/20221002001519/https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

* * *

TIL, or Today I Learned, is where our developers share the best tech stuff they found every day. You can find smart solutions for some issues, useful  <g class="gr_ gr_171 gr-alert gr_spell gr_inline_cards gr_disable_anim_appear ContextualSpelling ins-del multiReplace" id="171" data-gr-id="171"><g class="gr_ gr_178 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar multiReplace" id="178" data-gr-id="178">advices</g></g>  and anything which will make your developer life easier.

Photo by [Sara Riaño](http://web.archive.org/web/20221002001519/https://unsplash.com/photos/qoKTJo2zpRs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](http://web.archive.org/web/20221002001519/https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)