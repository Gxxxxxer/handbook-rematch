---
description: 除非您正在构建自己的插件或添加中间件，否则本节可能对您没有帮助。 有关常见init选项的列表，请参阅@rematch / core API
---

# Init Redux API

* init
  * redux
    * initialState
    * reducers
    * middlewares
    * enhancers
    * rootReducers
    * combineReducers
    * createStore
    * devtoolOptions

### Redux

本节提供对Redux设置的访问以及覆盖Redux方法的选项。

#### initialState

```javascript
init({
  redux: {
    initialState: any
  }
 })
```

你app的initialState。 这可能不是必需的，因为model的状态会覆盖init state。

#### reducers

```javascript
const someReducer = (state, action) => {
  switch(action.type) {
    default:
      return state
  }
}

init({
  redux: {
    reducers: {
      someReducer,
    }
  }
})
```

允许传递reducer functions，而不是model。 虽然不推荐，但可用于迁移Redux代码库或配置不同的Redux扩展。

#### middlewares

```javascript
init({
  redux: {
    middlewares: [customMiddleware()]
  }
})
```

添加middleware到你的store

#### enhancers

```javascript
init({
  redux: {
    enhancers: [customEnhancer()]
  }
})
```

添加enhancers到你的store。

#### rootReducers

```javascript
init({
  redux: {
    rootReducers: {
      'RESET': (state, action) => undefined,
    }
  }
})
```

一种设置中间件钩子的方法，它位于root reducer的基础上。 与中间件不同，返回值是下一个状态。如果**undefined**，这个state将会退回到reducers 初始state。

#### combineReducers

```javascript
init({
  redux: {
    combineReducers: customCombineReducers
  }
})
```

允许访问覆盖Redux的**combineReducers**方法。



