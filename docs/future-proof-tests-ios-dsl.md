# 通过添加 DSL 在 iOS 中编写面向未来的测试

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/future-proof-tests-ios-dsl>

 在编写测试时，重要的是我们的目标是测试行为，而不是测试实现。这有助于我们更快地发布，并防止人们在将来由于低可维护性而关闭测试。

这就是为什么以 DSL(领域特定语言)的形式添加一个抽象层是值得的。

说到这里，让我们根据自己的需要解开这个短语。我们将通过看一个例子来做这件事。想象一个有多个玩家的游戏，在游戏结束时，我们希望呈现一个有多个赢家的表格视图。

根据实现的不同，我们可能直接将它作为视图控制器的一部分，或者作为专用视图的一部分。让我们假设第一种情况:

```
final class WinnersViewControllerTests: XCTestCase {

func test_onLoad_showsAllWinners() {
// given:
 let sut = makeSUT(for: [winnerOne, winnerTwo, winnerThree])

 // when:
 sut.loadViewIfNeeded()

 // then:
 XCTAssertEqual(sut.tableView.numberOfRows(inSection: 0), 3)
}

// MARK: Helpers

private func makeSUT(for winners: [Winner] = []) -> WinnersViewController {
 return WinnersViewController(winners)
}

 private var winnerOne: Winner { Winner(name: "winner 1") }
 private var winnerTwo: Winner { Winner(name: "winner 2") }
 private var winnerThree: Winner { Winner(name: "winner 3") }
}
```

[测试](/web/20220926193341/https://www.netguru.com/blog/10-must-know-tips-ios-testing)看起来没问题。它很好地分隔了给定、何时、然后几个部分，并按预期覆盖了我们的 WinnersViewController。

一开始，我们说我们想测试行为而不是实现。然而，我们可以在测试中的什么地方测试实现呢？

看看这一行:

```
XCTAssertEqual(sut.tableView.numberOfRows(inSection: 0), 3)
```

如果我们可以只写:

```
XCTAssertEqual(sut.winnersDisplayed, 3)
```

我们如何做到这一点？

只需在测试文件的底部添加一个扩展名。

```
private extension WinnersViewController {
 var winnersDisplayed: Int {
 tableView.numberOfRows(inSection: 0)
 }
}
```

如果你有直觉，我们也应该对这个神奇的“0”数字做点什么，那就太好了！我们可以更进一步，在扩展中写入:

```
var winnersSection: Int { 0 }
```

我们上面的例子现在看起来像这样:

```
private extension WinnersViewController {
 var winnersDisplayed: Int {
 tableView.numberOfRows(inSection: winnersSection)
 }
}
```

现在，这就是我们所说的领域特定语言(DSL)。我们在测试中的思路不是针对当前的 UITableView 实现，而是针对预期的业务行为，这将显示获胜者。

此外，如果将来由于新的设计需求，我们想要改变集合视图的显示，我们只需要更新这个小属性，而不是，例如，在我们所有的测试中检查表视图。

干得好，你刚刚创建了自己的 DSL！

## 在 iOS 测试中创建 DSL

如果你想了解更多关于通过添加 DSL 在 [iOS](/web/20220926193341/https://www.netguru.com/blog/what-are-ios-memory-leaks-and-how-to-detect-them) 中编写未来验证测试的信息，请查看我们的资源 [Netguru iOS 良好实践。](http://web.archive.org/web/20220926193341/https://github.com/netguru/iOS-Good-Practices/tree/main/Articles/Unit%20Tests%20and%20Testability/DSL%20in%20BDD)为了进一步阅读，我们推荐乔恩·里德和 [Essential Developer](http://web.archive.org/web/20220926193341/https://www.essentialdeveloper.com/) 的 iOS 单元测试示例