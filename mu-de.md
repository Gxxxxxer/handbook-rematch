# 目的

Redux 是一个奇妙的状态管理工具，有健全的中间件生态与出色的开发工具。

Rematch 在 Redux 的基础上构建并减少了样板代码和执行了一些最佳实践。

说得清楚点，Rematch 移除了 Redux 所需要的这些东西：

* 声明 action 类型
* action 创建函数
* thunks
* store 配置
* mapDispatchToProps
* sagas

## Redux 与 Rematch 的对比

让 Redux 与Rematch 作对比有助于让理解更加清晰。

### Rematch

#### 1. model

```javascript

import { init } from '@rematch/core'const count = {  state: 0,  reducers: {    upBy: (state, payload) => state + payload  }}init({  models: { count }})
```

