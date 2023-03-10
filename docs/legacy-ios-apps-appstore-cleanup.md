# 处于危险中的传统 iOS 应用程序-检查您是否处于应用程序商店清理的风险中

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/legacy-ios-apps-appstore-cleanup>

 众所周知，苹果切断了他们对旧标准、设备和软件版本的支持。最近，谣言得到了证实——iPhone 7 不再有耳机插孔了。最新一代 MacBooks 中的所有端口都被更薄、更现代的 USB-C 类型端口所取代。连心爱的 MagSafe 电源接头都换成了 USB-C，这证明了苹果不怕做大决定。随着 App Store 的增长超过以往任何时候，废弃的遗留 iOS 应用程序集每年都在变得越来越大，这意味着苹果将不得不开始更好地维护它。

Big Cleanup in the App Store

 几个月前，苹果发布了 [App Store 改进](http://web.archive.org/web/20220926195602/https://developer.apple.com/support/app-store-improvements/) 页面，在这里他们宣布了 App Store 清理，其目的是让用户更容易找到应用。从页面上我们可以了解到，苹果基本上会删除那些过时的、不遵循审查指南的或者已经过时的应用。这一进程自 2016 年 9 月 7 日以来一直在进行。真正有趣的是，最近在 iOS 10.3 beta 1 版本中，一位 [开发者——彼得·斯坦伯格](http://web.archive.org/web/20220926195602/https://twitter.com/steipete/status/826417476426670081) - 发现了一条消息，警告用户他们正在使用的应用可能无法在未来版本的 iOS 中工作，需要由其开发者进行更新。

人们怀疑，随着预计将于今年秋天发布的 iOS 11 的推出，过时的应用程序(仅支持 32 位架构的应用程序)将会被删除。由于每年 6 月和 7 月之交都会发布最新 iOS 的测试版，所以没有太多时间进行大的更新，尤其是如果你的应用程序仍然存在并且拥有大量用户的话。苹果还告知[应用开发者](/web/20220926195602/https://www.netguru.com/services/ios-mobile-app-development)将被要求在 30 天内提交应用的更新。如果他们不这样做，应用程序将从商店中删除，直到更新提交并获得批准。

下一步做什么？

如果你[一年更新几次你的手机应用](http://web.archive.org/web/20220926195602/https://www.netguru.com/services/mobile-development)，没必要担心。然而，如果你不定期更新你的应用，最重要的是修改它，以确保它遵循 [应用商店审查指南](http://web.archive.org/web/20220926195602/https://developer.apple.com/app-store/review/guidelines/) 。任何违反这些规则的行为都将导致应用审查被拒绝，无论如何，你都需要调整应用以遵循审查指南。在提交更新之前，检查一下 [常见应用拒绝](http://web.archive.org/web/20220926195602/https://developer.apple.com/app-store/review/rejections/) 也会非常有用。我也很确定苹果会移除所有不支持 64 位架构的应用。

从技术的角度来看，你需要使用最新的开发者工具，例如 Xcode 8.3，包括最新的 SDK。使用最新的工具立即构建旧的应用程序可能是不可能的，并且可能需要一些开发时间来使项目适应当前的标准和配置需求。然而，最新的 Xcode 和 SDK 保证您的应用程序将支持 64 位架构。

传统项目本身可能会成为快速更新版本的障碍，但你也需要关注应用程序的其他方面。第一个是依赖兼容性。旧的应用程序是使用旧的依赖项构建的，这些依赖项很有可能很快就不再受支持，或者它们的新版本将在其 API 中包含重大更改。这也需要一些时间来改善或修复。实现这些变更所需的时间与项目当前的代码质量密切相关。代码越好，更新就越容易，所以不要忘记考虑这一点。

设计。一个优秀的用户界面是一个成功应用的关键因素。App Store 审核指南要求你提供一款提供良好 UX 的应用，并遵循 [人机界面指南](http://web.archive.org/web/20220926195602/https://developer.apple.com/ios/human-interface-guidelines/overview/design-principles/) ，所以要确保你的应用看起来很圆滑！如果你看到任何改进的空间，现在是让你的应用变得更好的最佳时机。

最后，代码本身。这些标准多年来一直在变化。现在的大部分 app 都是用 Swift 编写的，这带来了额外的好处，比如对开发更有信心，开发时间更短，bug 更少。它很现代，很好使用，不会很快过时，这意味着你可以安全地为你的应用添加新功能。新功能当然很重要，但也许你多年来经历的那些烦人的问题可以通过用 Swift 重构你的应用程序来解决？你可能要考虑转换到一个更现代的方法，让你的应用程序经得起未来的考验。

Finally, the code itself. The standards have changed over years. Most apps today are written in Swift, which brings extra benefits, such as more confidence in development, shorter development time and fewer bugs. It’s modern, it’s great to work with and it won’t get outdated quickly, which means that you can safely add new features to your app. New features are important, of course, but maybe those few nagging issues you have experienced over the years could be solved by refactoring your app with Swift? You might want to consider switching to a more modern approach and make your app future-proof.