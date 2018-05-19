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







