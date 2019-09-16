# Redux

## 一.设计思想
Redux 的设计思想很简单

```html
(1) Web 是一个状态机，视图和状态是一一对应的
(2) 所有的状态，保存在一个对象里面
```

## 三. 基本概念和API

### 3.1 Store

Store 是保存数据的地方，应用的data 可以从store 里面取出来
Redux 提供 createStore 这个函数，用来生成Store

```react.js
import { createStore } from Redux
const store = createStore（fn);
```

createStore 接受另一个函数作为参数，并且生成新的Store 对象

### 3.2 State

Store 包含所有的数据，但是如果得到某个时点的数据，我们要对Store 生成快照。

当前时刻的State， 可以通过store.getState()拿到。

```react.js
import { createStore } from 'redux';
const store = createStore(fn);

const state = store.getState();
```

在Redux 中， 一个State 对应一个View。 只要你知道State, 你就会知道View, 反之亦然

### 3.3 Action

State 的变化会导致View 的变化，但是用户接触不到State,只能接触到View, 所以State 的变化必须是View 导致。

Action 就是View 发出的通知，表示State 应该要发生变化。

Action 是一个对象，其中的type 属性是必须的，表示Action 的名称，其他属性可以自由设置。

```react.js
const action = {
  type: 'ADD_TODO',
  payload: 'Learn Redux'
}
```

Action 的名称是ADD_TODO 携带的字符是Learn Redux

### 3.4 Action Creator

View 要发送多少种信息，就会有多少种Action. 可以定义一个Action Creator 这样可以省事

```react.js
const ADD_TODO = '添加 TODO'；

function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}

const action = addTodo(‘Learn Redux’);
```

在这里，addTodo 函数就是一个Action Creator

### 3.5 store.dispatch()

在dispatch 是唯一一个可以把 Action 发送出去的人

Example:

```react.js
import { createStore } from ‘redux’；
const store = createStore(fn)；

store.dispatch({
  type: "ADD_TODO",
  payload: "Learn Redux"
});
```

结合Action Creator， 代码可以写：

```react.js
store.dispatch(addTodo("Learn Redux"));
```


### 3.6 Reducer
