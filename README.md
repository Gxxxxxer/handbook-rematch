---
description: >-
  Rematch是没有boilerplate的Redux最佳实践。没有多余的action types，action creators，switch
  语句或者thunks。
---

# Rethink Redux.

* [为什么我们创建Rematch](https://hackernoon.com/redesigning-redux-b2baee8b8a38)
* [Video: Rematch介绍](https://www.youtube.com/watch?v=3ezSBYoL5do)
* Redux与Rematch的比较

### 索引

* 快速开始
* 目的
* 例子
* 迁移指南
* API参考
  * 核心API
  * 初始Redux API
  * 插件API
* 技巧
  * Devtools
  * React
  * Vue
  * Testing
  * TypeScript
  * Immer
  * Decoupling reducers
* 插件
  * Selectors
  * Loading
  * Persist
  * Updated
  * React Navigation
  * Immer
*  灵感

### 快速开始

```bash
npm install @rematch/core
```

#### 第一步：Init

**init** 用来配置你 reducers, devtools & store。

index.js

```javascript
import { init } from '@rematch/core'
import * as models from './models'

const store = init({
  models,
})

export default store
```

_对于更高级的设置，查看插件和Redux配置选项。_

#### 第二步：Models

该model促使state， reducers， async actions 和 action creators 放在同一个地方。

models.js

```javascript
export const count = {
  state: 0, // initial state
  reducers: {
    // handle state changes with pure functions
    increment(state, payload) {
      return state + payload
    }
  },
  effects: {
    // handle state changes with impure functions.
    // use async/await for async actions
    async incrementAsync(payload, rootState) {
      await new Promise(resolve => setTimeout(resolve, 1000))
      this.increment(payload)
    }
  }
}
```

_查看_[_reducer文档_](https://github.com/rematch/rematch/blob/master/docs/api.md#reducers)_以了解更多信息，包括如何触发其他model的action。_

理解模型与回答几个问题一样简单：

1. 我的初始state是什么?**state**
2. 我如何改变state？**reducers**
3. 我如何处理异步action？  **effects** with async/await

#### Step 3: Dispatch

 **dispatch** 是我们如何在你的model中触发 reducers 和 effects。 Dispatch 标准化了你的action，而无需编写action types 或者  action creators。

```javascript
import { dispatch } from '@rematch/core'

                                                  // state = { count: 0 }
// reducers
dispatch({ type: 'count/increment', payload: 1 }) // state = { count: 1 }
dispatch.count.increment(1)                       // state = { count: 2 }

// effects
dispatch({ type: 'count/incrementAsync', payload: 1 }) // state = { count: 3 } after delay
dispatch.count.incrementAsync(1)                       // state = { count: 4 } after delay
```

 Dispatch 能被直接调用，或者用 dispatch\[model\]\[action\]\(payload\)简写。

#### Step 4: View

* Count:[ JS](https://codepen.io/Sh_McK/pen/BJMmXx?editors=1010) \| [React ](https://codesandbox.io/s/3kpyz2nnz6)\| [Vue](https://codesandbox.io/s/n3373olqo0) \| [Angular](https://stackblitz.com/edit/rematch-angular-5-count)
* Todos[: React](https://codesandbox.io/s/92mk9n6vww)

```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider, connect } from 'react-redux'
import store from './index'

const Count = props => (
  <div>
    The count is {props.count}
    <button onClick={props.increment}>increment</button>
    <button onClick={props.incrementAsync}>incrementAsync</button>
  </div>
)

const mapState = state => ({
  count: state.count
})

const mapDispatch = ({ count: { increment, incrementAsync }}) => ({
  increment: () => increment(1),
  incrementAsync: () => incrementAsync(1)
})

const CountContainer = connect(mapState, mapDispatch)(Count)

ReactDOM.render(
  <Provider store={store}>
    <CountContainer />
  </Provider>,
  document.getElementById('root')
)
```



