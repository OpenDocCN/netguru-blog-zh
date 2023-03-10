# 与 RxSwift 联网

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/networking-with-rxswift>

 如今，几乎每个应用程序都有某种服务器连接。在这个初学者的小教程中，我将向你展示如何使用 RxSwift 处理网络通信。出于本指南的目的，我们将创建一个使用 [Hipolabs API](http://web.archive.org/web/20220924170412/http://universities.hipolabs.com/) 搜索大学的小应用程序。网络通信的核心将基于`URLSession`。我假设你知道 iOS 编程的基础，所以我将只着重解释项目的 Rx 部分。

## 准备项目

我们旅程的第一步是准备项目，在 Xcode 中创建项目后，我们需要添加两个外部库:

我使用了那个 [Cocoapods](http://web.archive.org/web/20220924170412/https://cocoapods.org/) ，但是可以通过 [Carthage](http://web.archive.org/web/20220924170412/https://github.com/Carthage/Carthage) 或者手动随意导入库。有关说明，请访问 [RxSwift 知识库页面](http://web.archive.org/web/20220924170412/https://github.com/ReactiveX/RxSwift#installation)

### 简单布局

当我们的项目为编码做好准备时，我们需要创建一个显示接收数据的地方。为此，我在主`ViewController`中创建了简单的`UITableView`和`UISearchController`，它们应该嵌入到`UINavigationController`中

下面是完成这项工作的代码:

```
private let tableView = UITableView()
private let cellIdentifier = "cellIdentifier"

private let searchController: UISearchController = {
  let searchController = UISearchController(searchResultsController: nil)
  searchController.searchBar.placeholder = "Search for university"
  return searchController
}()

private func configureProperties() {
    tableView.register(TableViewCell.self, forCellReuseIdentifier: cellIdentifier)
    navigationItem.searchController = searchController
    navigationItem.title = "University finder"
    navigationItem.hidesSearchBarWhenScrolling = false
    navigationController?.navigationBar.prefersLargeTitles = true
}

private func configureLayout() {
    tableView.translatesAutoresizingMaskIntoConstraints = false
    view.addSubview(tableView)
    NSLayoutConstraint.activate([
        tableView.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor),
        tableView.leftAnchor.constraint(equalTo: view.safeAreaLayoutGuide.leftAnchor),
        tableView.rightAnchor.constraint(equalTo: view.safeAreaLayoutGuide.rightAnchor),
        tableView.bottomAnchor.constraint(equalTo: view.bottomAnchor)
    ])
    tableView.contentInset.bottom = view.safeAreaInsets.bottom
}
```

这是结果:

## 我们想要得到什么？我们希望如何接受它？

如果我们的布局准备好了，我们可以尝试处理来自 REST API 的一些数据。为此我们将使用 [Hipolabs API](http://web.archive.org/web/20220924170412/http://universities.hipolabs.com/) 。我们想从我们的`UISearchController`中获得包含搜索短语的大学信息。

### 例子

这里有一个用`middle`作为名称参数查找大学的请求和响应的例子。

| `http://universities.hipolabs.com/search?name=middle` |
| **响应** |
| `[{"name": "Middlesex University", "domains": ["mdx.ac.uk"], "web_pages": ["http://www.mdx.ac.uk/"], "alpha_two_code": "GB", "state-province": null, "country": "United Kingdom"}, ...]` |

现在，当我们知道 API 如何工作时，我们就可以创建请求和模型对象了。

### 模型

对于来自服务器的数据，我们可以使用类似于`[String: Any]`的 JSON 字典，但我更喜欢创建更清晰、更易于使用的数据模型。为了接收大学对象，我创建了 struct `UniversityModel`，它符合`Codable`协议，因此我们不需要为解析数据而烦恼，让我们把它留给 swift 引擎吧。

```
struct UniversityModel: Codable {
    let name: String
    let webPages: [String]?
    let country: String

    private enum CodingKeys: String, CodingKey {
        case name
        case webPages = "web_pages"
        case country
    }
}
```

### 要求

为了使这更通用，我们需要创建`APIRequest`协议，这样不同的请求可以由同一个`APIClient`来处理。

`APIRequest`类由两部分组成:

协议本身，其中定义了必要的属性:

```
protocol APIRequest {
    var method: RequestType { get }
    var path: String { get }
    var parameters: [String : String] { get }
}
```

将从`APIRequest`的实例创建`URLRequest`的协议扩展:

```
extension APIRequest {
    func request(with baseURL: URL) -> URLRequest {
        guard var components = URLComponents(url: baseURL.appendingPathComponent(path), resolvingAgainstBaseURL: false) else {
            fatalError("Unable to create URL components")
        }

        components.queryItems = parameters.map {
            URLQueryItem(name: String($0), value: String($1))
        }

        guard let url = components.url else {
            fatalError("Could not get url")
        }

        var request = URLRequest(url: url)
        request.httpMethod = method.rawValue
        request.addValue("application/json", forHTTPHeaderField: "Accept")
        return request
    }
}
```

我还在`APIRequest`类中创建了一个小枚举来改进 httpMethod 的声明:

```
public enum RequestType: String {
    case GET, POST
}
```

当协议准备好了，我们就可以提出真正的请求，通过名字来搜索大学。为此，我们需要创建另一个从`APIRequest`协议继承的类，其中定义了方法、端点路径和参数。

```
class UniversityRequest: APIRequest {
    var method = RequestType.GET
    var path = "search"
    var parameters = [String: String]()

    init(name: String) {
        parameters["name"] = name
    }
}
```

好吧，有很多，但是这个 RxSwift 在哪里？

## 魔法时间到

现在是拼图中最重要的一块的时候了，这部分将改变我们对服务器数据的请求。现在是`APIClient`的时候了！

`APIClient`是一个类，我们将从 RxCocoa 使用`URLSession`上的`rx`扩展发出请求，然后`map`将响应数据发送到已经解析的数据模型(如果只有模型是`Codable`)。`.data(request: URLRequest)`函数将确保我们的状态代码是`200..<300`，然后返回数据，如果不是，那么它将抛出一个错误。

```
class APIClient {
    private let baseURL = URL(string: "http://universities.hipolabs.com/")!

    func send<T: Codable>(apiRequest: APIRequest) -> Observable<T> {
        let request = apiRequest.request(with: baseURL)
        return URLSession.shared.rx.data(request: request)
            .map { 
                try JSONDecoder().decode(T.self, from: data)
            }
            .observeOn(MainScheduler.asyncInstance)
    }
}
```

## 还有一件事...

创建完`APIClient`后，最后一部分是将所有东西连接在一起。

我们期望的结果:

**在搜索字段中键入搜索短语** → **用搜索短语** → **创建的请求实例大学模型数组** → **刷新`UITableView`用新数据**填充，所有这些都在**的 10 行中！！！**

```
searchController.searchBar.rx.text.asObservable()
  .map { ($0 ?? "").lowercased() }
  .map { UniversityRequest(name: $0) }
  .flatMapLatest { [unowned self] request -> Observable<[UniversityModel]> in
    return self.apiClient.send(apiRequest: request)
  }
  .bind(to: tableView.rx.items(cellIdentifier: cellIdentifier)) { index, model, cell in
    cell.textLabel?.text = model.name
  }
  .disposed(by: disposeBag)
```

请记住导入`RxSwift`和`RxCocoa`并创建两个变量:

```
private let apiClient = APIClient()
private let disposeBag = DisposeBag()
```

如果你想在用户点击单元格时呈现一个大学网站，你可以通过利用模型和反应式绑定，用 7 行代码来实现。

```
tableView.rx.modelSelected(UniversityModel.self)
  .map { URL(string: $0.webPages?.first ?? "")! }
  .compactMap { URL(string: $0) }
  .map { SFSafariViewController(url: $0) }
  .subscribe(onNext: { [weak self] safariViewController in
    self?.present(safariViewController, animated: true)
  })
  .disposed(by: disposeBag)
```

## **最终效果**

今天到此为止。你可以在这个[库](http://web.archive.org/web/20220924170412/https://github.com/saoth/Networking-with-RxSwift)找到整个项目。我希望你喜欢。