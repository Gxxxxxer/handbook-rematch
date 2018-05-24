---
description: 给Rematch使用的selectors插件
---

# Rematch Select

### 安装

```bash
npm install @rematch/select
```

> 对于@rematch/core@0.x 使用 @rematch/select@0.3.0

### 设置

```javascript
import selectorsPlugin from '@rematch/select'
import { init } from '@rematch/core'

const select = selectorsPlugin()

init({
  plugins: [select]
})
```

### selectors

`selectors: { [string]: (state, ...params) => any }`

selectors是只读状态的片段。

```javascript
{
  name: 'cart',
  state: [{
    price: 42.00,
    amount: 3,
  }],
  selectors: {
    total(state) {
      return state.reduce((a, b) => a + (b.price * b.amount), 0)
    }
  }
}
```







