# Decoupling reducers

Rematch 鼓励你保持一个扁平的状态来解决若干问题。每个 model 都应该被视为一个独立的实体，并且只应该通过它的 reducer 进行更新，这就是为什么 Rematch 不允许嵌套 reducer。

这就是说，有些情况下嵌套 reducer 可能是件好事。在这个技巧中，我们将看到如何使用可重复使用的reducer 函数。

我们从一个例子开始。 在Redux中，当我们想要存储有序列表时，常见模式是“allIds，byId”。将实体存储为数组需要花费额外的代价来遍历数组以找到我们的目标。另一方面，将实体存储为对象不会保留项目的顺序。 所以我们两者都用：

```javascript
const todoList = {
  state: {
    byId: {
      0: {
        task: 'Learn Rematch',
        isDone: true
      },
      1: {
        task: 'Learn functional programming',
        isDone: false
      }
    },
    allIds: [1, 0]
  }
}
```

`byId`键存储实体，而`allIds`则跟踪实体的顺序。

这造成了另一个问题：

* `byId`和`allIds`指的是同一个实体，他们肯定需要生活在同一个model中
* `byId`和`allIds`互不依赖，他们肯定需要分开处理

让我们试着看看我们能得到什么而不去解耦我们的reducer:

```javascript
const todoList = {
  state: {...},
  reducers: {
    // update allIds and byId
    remove(state, payload) {
      const { idToRemove } = payload;
      return {
        byId: Object.entries(state.byId)
          .filter(([id, todo]) => id !== idToRemove)
          .reduce((acc, [id, todo]) => ({ ...acc, [id]: todo }), {}),
        allIds: state.allIds.filter(id => id !== idToRemove)
      };
    },
    // update byId only
    toggle(state, payload) {
      const { idToToggle } = payload;
      return {
        ...state, // don't update allIds,
        byId: {
          ...state.byId,
          [idToToggle]: {
            ...state.byId[idToToggle],
            isDone: !state.byId[idToToggle].isDone
          }
        }       
      };
    }
  }
};
```

 [React example](https://codesandbox.io/s/lry6024mkl)

我们看到我们的reducer开始变得大而不可读。希望我们可以将更新函数分离出来。

我们可以开始分离我们纯粹的可重用函数

```javascript
function filterObjectByKey(obj, f) {
  return Object.entries(obj)
    .filter(([key, value]) => f(key))
    .reduce((acc, [key, value]) => ({ ...acc, [key]: value }), {});
}
```

现在我们可以分开我们的`reducer functions`，该函数更新状态一部分。

```javascript
function removeById(state, payload) {
  const { idToRemove } = payload;
  return filterObjectByKey(state, id => id !== idToRemove);
}

function removeAllIds(state, payload) {
  const { idToRemove } = payload;
  return state.filter(id => id !== idToRemove);
}

function toggleById(state, payload) {
  const { idToToggle } = payload;
  return {
   ...state,
   [idToToggle]: {
     ...state[idToToggle],
     isDone: !state[idToToggle].isDone
   }
  }
}

function toggleAllIds(state, payload) {
  return state;
}
```

最后，我们通过将一部分state分配给我们的 reducers functions，最终将所有 state 合并在一起：

```javascript
const todoList = {
  state: {...},
  reducers: {
    remove(state, payload) {
      return {
        byId: removeById(state.byId, payload),
        allIds: removeAllIds(state.allIds, payload)
      };
    },
    toggle(state, payload) {
      return {
        byId: toggleById(state.byId, payload),
        allIds: toggleAllIds(state.allIds, payload)
      };
    }
  }
};

```

 [React example](https://codesandbox.io/s/x2r7nryn24)

Reducer functions 使我们的模型更简单，更易读。







