# React 和 Rx.js -可观察的力量(常见问题解答)

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/react-rxjs>

 RxJS 是什么？

RxJS 是一个库，用于通过使用可观察序列来编写异步和基于事件的程序。它提供了一个核心类型，即[可观察的](http://web.archive.org/web/20221007150033/https://rxjs-dev.firebaseapp.com/guide/observable)，卫星类型(观察者、调度器、主体)，以及受 Array#extras 启发的操作符(map、filter、reduce、every 等)。)来允许将异步事件作为集合来处理。

ReactiveX 将[观察者模式](http://web.archive.org/web/20221007150033/https://en.wikipedia.org/wiki/Observer_pattern)与[迭代器模式](http://web.archive.org/web/20221007150033/https://en.wikipedia.org/wiki/Iterator_pattern)和[函数式编程与集合](http://web.archive.org/web/20221007150033/http://martinfowler.com/articles/collection-pipeline/#NestedOperatorExpressions)相结合，以满足管理事件序列的理想方式的需求。

RxJS 中解决异步事件管理的基本概念如下。

*   **可观察:**代表未来值或事件的可调用集合的想法。
*   **Observer:** 是一个回调集合，它知道如何监听被观察对象传递的值。
*   **认购:**代表可观察的执行；它主要用于取消执行。
*   **操作符:**是纯函数，支持函数式编程风格，通过映射、过滤、连接、归约等操作来处理集合。
*   **Subject:** 相当于 EventEmitter，是向多个观察者多播值或事件的唯一方式。

**调度器:**是控制并发性的集中调度器，允许我们在计算发生时进行协调，例如 setTimeout 或 requestAnimationFrame 或其他。

## 去哪里学 Rx？

第一个电话肯定是去 RxJS 主网页:

文档非常详细，并且呈现得很好，大部分概念都涉及得很深，这很好，但有时会使人很难快速理解。

因此，当我阅读一些更复杂的概念时，我发现有更多的首次 rx 用户友好教程:

*   [https://www.learnrxjs.io/](http://web.archive.org/web/20221007150033/https://www.learnrxjs.io/)(这对于学习或快速检查如何与一些接收操作员合作特别有用)

至于将 [React](/web/20221007150033/https://www.netguru.com/blog/next-js-vs-react) 与 Rx 结合起来，有很多文章你可能想看看:

其他文章:

## 我可以用 Rx 做什么？

*   RxJS 非常适合复杂的异步查询和对事件的反应。使用 RxJS 和 lodash，在这些情况下很容易编写干净的代码。
*   您可以使用 RxJS 来处理和限制用户事件，从而更新应用程序的状态。
*   有了 React，它非常适合在组件之间创建通信。
*   它提供了允许开发人员创建数据流并对这些数据流进行操作的方法。有如此多的操纵或创造的选择，所以你有一个大的领域可以发挥(例如，你可以使用它与 Websockets)。
*   对于任何涉及时间概念的工作，或者当您需要推理可观测的历史值/事件(而不仅仅是最新的)时，推荐使用 RxJS，因为它提供了更多的底层原语。

## 用 Rx 不爽的时候？

*   维护——你的团队对 Rx 有经验吗？如果没有，新人可能很难学习 Rx js，因为它有一个相当陡峭的学习路径。
*   调试起来相当困难。记录器有时只显示源代码深处的函数，所以您需要深入研究——或者只是猜测——哪里出错了。
*   此外，对于某些项目来说，这可能有些矫枉过正。
*   它也有优点和缺点，但是有如此多的操作符，您可以在一个管道中组合，并且很容易迷路(调试)。

## 我可能会陷入哪些问题？

RxJS 的问题可能是它只是一个实用程序，它没有向人们展示如何设计他们的应用程序，以便解决一些常见的难题:

*   检查应用程序的当前状态，
*   组合其值无序的流，
*   组合其值可能不完整的流，
*   处理超时，
*   处理过时的流数据，
*   取消飞行中的流操作，
*   流的条件行为，
*   重试机制。

由于 RxJS 是在一元编程之后建模的，如果架构正确，所有这些都可以解决，但是如果没有适当的经验，它们也不是微不足道的。

## MobX 是不是已经不像 Rx store 实现了？

MobX 是状态管理器，RxJS 是处理异步事件的库。

至于 MobX，文档说:

***MobX 可以和 RxJS 结合吗？***

*是的，你可以从 mobx-utils* *中使用* [*toStream 和 fromStream 来使用 RxJS 和其他与 mobx 兼容的 TC 39 observable。*](http://web.archive.org/web/20221007150033/https://github.com/mobxjs/mobx-utils#tostream)

***什么时候用 RxJS 代替 MobX？***

对于任何涉及明确使用时间概念的工作，或者当你需要推理一个可观察的历史值/事件(而不仅仅是最新的)时，推荐使用 RxJS，因为它提供了更多的底层原语。每当您想对状态而不是事件做出反应时，MobX 提供了一种更简单、更高级的方法。实际上，结合 RxJS 和 MobX 可能会产生真正强大的结构。例如，使用 RxJS 来处理和抑制用户事件，并作为更新状态的结果。如果 MobX 可以观察到状态，它就会相应地更新 UI 和其他派生。

将 MobX 与 RxJS 一起使用可能也是一个好主意，但是您将需要安装一些其他的依赖项。

## 与 RxJS 反应:

在根项目目录中安装必要的依赖项:

```
npm install rxjs
npm install rxjs-compat
```

让我们关注一个真实的例子，一个 fetch 调用需要开发人员执行多个数据操作步骤和额外的 API 调用来获取加载应用程序所需的数据。

这是我连接到 redux 状态的主要组件；

```
 class Main extends Component {
  subscription;

  constructor() {
    super();
    this.search$ = new Subject();
    this.search = this.search$.asObservable().pipe(
      debounceTime(500),
    );
  }

  componentDidMount() {
    this.subscription = this.search.subscribe((text) => {
      this.callApiToGetHeroes(text);
    });
  }

  componentWillUnmount() {
    this.subscription.unsubscribe();
  }

  onSearch = (e) => {
    this.search$.next(e.target.value);
  }

  render() {
    const { listOfHeroes, team } = this.props;
    return (
      <div className="main_view">
        <Header />
        <SearchBar onSearch={this.onSearch} />
        <div className="main_view_list_container">
          <HeroList heroesList={listOfHeroes} />
          <ChosenList chosenList={team} />
        </div>
      </div>
    );
  }
}

Main.propTypes = {
  listOfHeroes: PropTypes.arrayOf(PropTypes.object),
  team: PropTypes.arrayOf(PropTypes.object),
  getHeroes: PropTypes.func,
};

Main.defaultProps = {
  listOfHeroes: [],
  team: [],
  getHeroes: null,
};

const mapStateToProps = (state) => ({ team: state.team, listOfHeroes: state.list });

const mapDispatchToProps = (dispatch) => ({
  getHeroes: () => dispatch(getHeroesAction()),
});

export default connect(mapStateToProps, mapDispatchToProps)(Main);
```

为了在主组件中创建反应性输入，它需要有一个可观察的创建者——在本例中是一个主体。

为了抑制 API 请求，我在 observable 中添加了 debounceTime(500 ),这样它将在下一次调用之前等待 500 ms。

主题只是创造可观察事物的一种方式。

**重要通知:**unsubscribe()observable on component will unmount 用于防止组件被破坏后，组件外还存在 observable，这样可以避免内存泄漏。

API 调用:

```
import 'babel-polyfill';
import { fromFetch } from 'rxjs/fetch';
import {
  switchMap,
  map,
  tap,
  flatMap,
} from 'rxjs/operators';
import { forkJoin } from 'rxjs';
import 'rxjs/add/observable/of';

import { ADD_TO_CHOSEN, REMOVE_FROM_CHOSEN, FETCH_HEROES_SUCCESS } from './types';

const fetchUrl = 'https://swapi.co/api/people/';
const imgUrl = 'https://i.pravatar.cc/200?img=';

export const addHero = (hero) => ({ type: ADD_TO_CHOSEN, hero });
export const removeHero = (hero) => ({ type: REMOVE_FROM_CHOSEN, hero });
export const getHeros = (heros) => ({ type: FETCH_HEROES_SUCCESS, heros });

const getWorlds = (list) =>
  forkJoin(list.map((hero) =>
   fromFetch(hero.homeworld)
      .pipe(
        switchMap((resp) => resp.json()),
        map((resp) => ({ ...hero, homeworld: resp.name })),
      )));

const getSpecies = (list) =>
 forkJoin(list.map((hero) =>
  fromFetch(hero.species[0])
      .pipe(
        switchMap((resp) => resp.json()),
        map((resp) => ({ ...hero, species: [resp.name] })),
      )));

const handleResponse = (resp) =>{
   if (resp.ok) {
     return resp.json();
   } else {
     return of({ error: true, message: `Error ${resp.status}` });
   }
}

export const getHeroesAction = () =>
   (dispatch) =>
    fromFetch(fetchUrl)
      .pipe(
        switchMap((response) => handleResponse(response)),
        map((response) => response.results),
        flatMap((response) => getWorlds(response)),
        flatMap((response) => getSpecies(response)),
        tap((completeHeroes) => dispatch(getHeros(completeHeroes))),
        catchError((err) => {
          console.error(err);
          return of({ error: true, message: err.message });
        }),
      );
```

因此，我将尝试解释那里发生了什么[(单击此处查看运营商文档以获得完整描述)](http://web.archive.org/web/20221007150033/https://www.learnrxjs.io/ "https://www.learnrxjs.io/")

我们从 **fromFetch** 开始，它将基本的 Fetch 调用转换为可观察的<响应> ( <可观察的>类型)它也可以用 **fromPromise** 来完成(不推荐用于版本< 6)。)或中的**。**

**管道**操作符允许在 observable 用一些 Rx 操作符发出之前操纵数据流。在这种情况下， **switchMap** 处理来自服务器的响应(它也取消以前的可观察性)。而**映射**操作符允许转换可观察值。

[(如果 Rx 地图有点混乱，请点击这里)](http://web.archive.org/web/20221007150033/https://blog.angular-university.io/rxjs-higher-order-mapping/ "https://blog.angular-university.io/rxjs-higher-order-mapping/")或[点击这里](http://web.archive.org/web/20221007150033/https://medium.com/@luukgruijs/understanding-rxjs-map-mergemap-switchmap-and-concatmap-833fc1fb09ff "https://medium.com/@luukgruijs/understanding-rxjs-map-mergemap-switchmap-and-concatmap-833fc1fb09ff")

接下来，Rx 魔术开始了——问题是响应数据的结构——hero object 中的一些值只是从另一个源获取值的 urls 使用 Rx，我们可以在一个流中完成所有工作！< B

forkJoin 的工作方式有点像 promise . all——它加入并等待所有内部可观察对象发出，然后返回新的可观察对象以及来自内部可观察对象的数据。它非常适合有多个 http 请求的情况。

**flatMap** 在这里用于展平用 forkJoin 创建的可观察对象，并在流上继续；

接下来的两个操作非常简单- **点击**

**catchError** 允许捕捉错误并以我们想要的方式转换它。

## 测试呢？

嗯，这非常简单——只是为了测试 react 应用程序中的一些流，我认为最好的方法是将我们的管道从组件中取出，并与其他操作符一起保存在一个单独的文件中——然后测试每个流的效果就变得非常容易了。仅仅举个例子:

```
 export const simpleMapTest = (observable$) => {
  return observable$.pipe(
    map(data => data + ' search'),
  )
};
```

和测试:

```
it('should change value of the observable', (done) => {
  const observable$ = of('text');
  simpleMapTest(observable$).subscribe((value) => {
    expect(value).toBe('text search');
    done();
  });
});
```

至于测试组件，我们不能真的订阅组件观察值，但是我们可以测试我们的流对这个组件的影响。让我们从前面创建的输入中取出去抖操作符。这个动作的测试应该是这样的:

```
it('should execute call API to get Heroes only once', () => {
  const wrapper = shallow(<MainComponent />);
  const shallowComponent = wrapper.instance(); shallowComponent.callApiToGetHeroes = jest.fn(); wrapper.update(); shallowComponent.onSearch({ target: { value: 'text' } }); shallowComponent.onSearch({ target: { value: 'text1' } }); shallowComponent.onSearch({ target: { value: 'text3' } });
  setTimeout(() => {
    expect(shallowComponent.callApiToGetHeroes).toHaveBeenCalledTimes(1);
    expect(shallowComponent.callApiToGetHeroes).toHaveBeenCalledWidth('text3');
  }, 0);
});
```

## 概要:

对于有经验的开发人员来说，Rx.js 是一个很好的工具，它可以很容易地在 React 环境中实现，但缺点是对于从未在任何项目中使用过 Rx 的人来说，第一次接触 React 方法可能是一件很困难的事情。

Rx.js 远不止本文涵盖的主题，所以我鼓励大家做一些研究，在掌握了基本概念之后，你可以[进入 rx.js 弹珠](http://web.archive.org/web/20221007150033/https://rxmarbles.com/ "https://rxmarbles.com/")。

图片由 [Artem Sapegin](http://web.archive.org/web/20221007150033/https://unsplash.com/@sapegin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](http://web.archive.org/web/20221007150033/https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)