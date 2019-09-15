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
