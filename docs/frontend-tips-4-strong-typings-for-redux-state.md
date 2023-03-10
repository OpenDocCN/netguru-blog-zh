# 前端快速提示# 4 Redux 状态的强类型

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/frontend-tips-4-strong-typings-for-redux-state>

 没有人喜欢那些大文章——这就是为什么我们创建快速提示——从你阅读它们的那一刻起改变你的开发者生活的简短提示。这些可能是 JS 代码中解释的一些模式，或者是一些更好的代码的技术。

Problem

您希望使用 [TypeScript](http://web.archive.org/web/20220925214534/https://www.typescriptlang.org/) 对您的 [Redux](http://web.archive.org/web/20220925214534/https://redux.js.org/) 状态进行强类型化，并与 IDE 集成自动完成、实时错误和更容易重构等特性。

## 解决办法

我们可以使用类型安全动作 ( [链接](http://web.archive.org/web/20220925214534/https://github.com/piotrwitek/typesafe-actions) ) ，一个由 [Piotr Witek](http://web.archive.org/web/20220925214534/https://github.com/piotrwitek) 创建和维护的包。它是一组帮助函数，允许我们轻松地创建和管理 Redux 状态。

### 创建根缩减器

src/store/rootReducer.ts

给定当前状态树和要处理的动作，reducer 是返回下一个状态树的函数。根变径管是应用中所有变径管的组合。

```
import { combineReducers } from "redux";
import gridReducer from "./ducks/grid/reducer";

const rootReducer = combineReducers({
  grid: gridReducer
});

export default rootReducer;
```

我们可以通过使用 StateType([documentation](http://web.archive.org/web/20220925214534/https://github.com/piotrwitek/typesafe-actions#statetype))助手来生成状态树形状类型，该助手从 reducer 函数中推断状态对象类型。

```
import { StateType } from "typesafe-actions";

type RootState = StateType<typeof rootReducer>
// { grid: GridStateShape }
```

### 创建助手类型

您可以定义全局类型并扩展默认的类型安全操作定义，以实现更好的 IDE 集成和自动完成。

### 创建动作创建者

src/store/ducks/grid/action creators . ts

typesafe-actions 公开一个 createAction 帮助器。它会自动创建一个标准化的动作创建器，你可以在调度功能中使用它。

### 创建一个减速器

src/商店/鸭子/网格/减速器. ts

typesafe-actions 公开了一个 createReducer 帮助器来创建 typesafe reducers。给定一个动作创建者，您可以分配接受状态和动作参数的动作处理程序，这些参数的类型将被自动缩小。

请注意，您可以始终使用标准的开关/条件缩减器，并通过使用 getType([documentation](http://web.archive.org/web/20220925214534/https://github.com/piotrwitek/typesafe-actions#gettype))助手来缩小类型: