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

State 收到 Action 之后 会 update State， 这样View 才会有变化。 State 的变化 叫做Reducer

Reducer 是一个函数， 它接受Action 和当前的State 作为参数，返回一个新的State

```react.js
const reducer = function (state, action) {
  // ...
  return new_state;
}
```

整个应用的厨师状态，可以作为State的初始值，下面是一个实际的例子

```react.js
const defaultState = 0;
const reducer = (state = defualtState, action) => {
  switch(action.type) {
    case 'ADD':
       return state + action.payload;
    default:
       return state;
  }
};

const state = reducer(1, {
  type: 'ADD',
  payload: 2
})
```

在 createStore 接受 Reducer 作为 参数，生成一个新的Store. 以后每当 store.dispatch 发送过来一个新的Action, 就会自动调用Reducer, 得到一个新的State

为什么叫做Reducer？ 因为他可以作为数组的reduce 的方法参数，可以看一下的一个例子

```react.js
const actions = [
  {type: 'ADD', payload: 0},
  {type: 'ADD', payload: 1},
  {type: 'ADD', payload: 2}
];

const total = actions.reduce(reducer, 0); //3
```

上面的代码中， action 有三个Action, 分别是0，1，2. reduce 方法接受Reducer 函数作为参数，就可以直接得到最终的状态3.

### 3.7 纯函数
Reducer 函数最重要的特征是，他是一个纯函数。也就是说， 只要是同样的输入， 比兴得到同样的输出。
纯函数是函数式编程的改变。

```
- 不得改写参数
- 不能调用系统I/O 的API
- 不能调用Date.now() 或者 Math.random（）等不纯的方法， 因为每次回得到不一样的结果。
```

由于Reducer 是纯函数， 可以办证用眼的State, 必定得到同样的View, SO Reducer 不能改变State

```react.js
function reducer(state, action) {
  return Object.assign({}, state,{ thingToChange });
  //或者
  return {...state, ...newState};
}

//State 是一个数组
function reducer（state, action） {
  return [...state，newItem]；
}
```

把State 的对象编程只读是没有办法的，如果要得到新的State, 唯一办法就是生成新的对象，这样的好处就是，任何时候，和某个View 对应的State 总是一个不变的对象。


### 3.8 store.subscribe()

Store
