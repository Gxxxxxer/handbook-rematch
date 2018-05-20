# @rematch/core API

```javascript
import { init, dispatch, getState } from '@rematch/core'
```

### init

`init(config)`

该函数被调用去设置Rematch。返回`store`。

```javascript
import { init } from '@rematch/core'

const store = init()
```

Init也可以通过下面的配置选项来调用。

#### models

`init({ models: { [string]: model } })`

```javascript
import { init } from '@rematch/core'

const count = {
  state: 0,
}

init({
  models: {
    count
  }
})
```

对于较小的项目，建议你保存model在一个“models.js”文件中并且命名导出他们。

```javascript
export const count = {
  state: 0,
}
```

对于较大的项目，建议你保存model在一个“models”文件夹中并且导出他们。

```javascript
// models/count.js
export default {
  state: 0,
}
```

```javascript
// models/index.js
export { default as count } from './count'
export { default as settings } from './settings'
```

这些可以使用`* as alias`语法导入。

```javascript
import { init } from '@rematch/core'
import * as models from './models'

init({ models })
```

**state**

 `state: any` Required

 该model的初始state

```javascript
const example = {
  state: { loading: false }
}
```

**reducers**

 `reducers: { [string]: (state, payload) => any }`

一个改变该model state的所有函数的对象。这些函数采用model的上一次state和一个payload作为形参，并且返回model的下一个装态。这些应该是仅依赖于state和payload参数来计算下一个state的纯函数。对于依赖“outside world”的代码（不纯的函数，如API调用等）的代码，请使用 effects。

```javascript
{
  reducers: {
    add: (state, payload) => state + payload,
  }
}
```

通过列出'model名字' + 'action名字'来作为key，Reducers也可以监听来自于其它model的action。

```javascript
{
  reducers: {
    'otherModel/actionName': (state, payload) => state + payload,
  }
}
```

**effects**

`effects: { [string]: (payload, rootState) }`

一个可以处理该model world outside功能（所有函数）的对象。

```javascript
{
  effects: {
    logState(payload, rootState) {
      console.log(rootState)
    }
  }
}
```

当与`async/await`一起使用时，Effects提供了一种处理异步action的简单方法。

```javascript
{
  effects: {
    async loadData(payload, rootState) {
      // wait for data to load
      const response = await fetch('http://example.com/data')
      const data = await response.json()
      // pass the result to a local reducer
      dispatch.example.update(data)
    }
  }
}
```

#### plugins {#plugins}

```text
init({
  plugins: [loadingPlugin, persistPlugin],
})
```

Plugins 是用来自定义init配置或内部hooks，它能添加功能到你的Rematch设置当中来。

