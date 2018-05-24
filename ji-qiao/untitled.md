# Devtools

### Redux-Devtools

Rematch具有开箱即用的[Redux Devtools](https://github.com/zalmoxisus/redux-devtools-extension)。不需要配置。

```javascript
init() // devtools up and running
```

还可以添加redux devtools[配置选项](https://github.com/zalmoxisus/redux-devtools-extension/blob/master/docs/API/Arguments.md)。

```javascript
init({
  redux: {
    devtoolOptions: options,
  },
})
```

### Reactotron

还可以设置 Rematch 与 [Reactotron devtools](https://github.com/infinitered/reactotron)一起工作。

```javascript
// Reactotron.config.js
import Reactotron from 'reactotron-react-native'
import { reactotronRedux } from 'reactotron-redux'

export default Reactotron
  .configure({
    name: 'MyAwesomeApp'
  })
  .use(reactotronRedux())
  // add other devtools here
  .connect()
```







