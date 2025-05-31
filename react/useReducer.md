
`useReducer` 是 React 提供的一个 Hook，适合用于管理**复杂状态逻辑**（尤其是多个子值互相影响）或需要对状态变更进行**清晰归类**的场景。它的用法类似于 [Redux](react/redux) 的 `reducer` 概念。

```
const [state, dispatch] = useReducer(reducer, initialState, init);
```

- **`reducer`**：一个函数 `(state, action) => newState`
    
- **`initialState`**：初始状态
    
- **`state`**：当前状态
    
- **`dispatch`**：用于分发 action，触发 `reducer` 执行
	
-  `init`： 
	- 懒初始化函数 
	- 当你的初始状态计算过程较复杂时，可以使用 `initFunction` 延迟计算初始值；
	- `initFunction` 只在组件挂载时运行一次；
	- 它接收 `initialArg` 作为参数，返回真正的初始 `state`。


## 示例：计数器

```
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error('Unknown action');
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
    </div>
  );
}
```


## 复杂点的例子

```
const initialState = {
  username: '',
  email: '',
  errors: {},
  loading: false,
  submitted: false,
};

function formReducer(state, action) {
  switch (action.type) {
    case 'SET_FIELD':
      return {
        ...state,
        [action.field]: action.value,
      };
    case 'SET_ERRORS':
      return {
        ...state,
        errors: action.errors,
      };
    case 'SUBMIT_START':
      return {
        ...state,
        loading: true,
        submitted: false,
      };
    case 'SUBMIT_SUCCESS':
      return {
        ...state,
        loading: false,
        submitted: true,
      };
    default:
      return state;
  }
}

// ......

const [state, dispatch] = useReducer(formReducer, initialState);

<input
  value={state.username}
  onChange={e =>
    dispatch({ type: 'SET_FIELD', field: 'username', value: e.target.value })
  }
/>
```


## useReducer 结合 useContext 实现组件间的通信

```
// couterStore.js

import React, { createContext, useReducer, useContext } from 'react';

const CounterContext = createContext();

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

export function CounterProvider({ children }) {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <CounterContext.Provider value={{ state, dispatch }}>
      {children}
    </CounterContext.Provider>
  );
}

export function useCounter() {
  return useContext(CounterContext);
}
```

使用组件通信：

```
import { useCounter } from './couterStore.js'

function ComponentA() {
  const { state, dispatch } = useCounter();
  return (
    <div>
      <h2>Component A</h2>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
    </div>
  );
}

function ComponentB() {
  const { state } = useCounter();
  return (
    <div>
      <h2>Component B</h2>
      <p>Count: {state.count}</p>
    </div>
  );
}

```

顶层：

```
import { CounterProvider } from './couterStore.js'
// ......

function App() {
  return (
    <CounterProvider>
      <ComponentA />
      <ComponentB />
    </CounterProvider>
  );
}
```