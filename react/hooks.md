目录：react/hooks

React Hooks 是 React 16.8 引入的重要特性，它为函数组件带来了状态管理和副作用处理等能力。


## react有哪些内置hooks

[useState](react/useState), [useRef](react/useRef), [useEffect](react/useEffect), [useLayoutEffect](react/useLayoutEffect), [useMemo](react/useMemo), [useContext](react/useContext), [useReducer](react/useReducer), [useSyncExternalStore](react/useSyncExternalStore)

## 优点

### 1. 函数组件增强：让函数组件具备类组件能力

- 在没有 Hooks 之前，函数组件无法使用 state、生命周期等功能。
- 使用 Hooks 后，可以在函数组件中管理状态、使用副作用、引用 DOM 等。
- 
### 2. 逻辑复用更灵活：自定义 Hook 替代 HOC/Render Props

- 自定义 Hook（如 `useAuth()`、`useScroll()`）使得逻辑复用更清晰，避免组件嵌套地狱。
- 比传统的高阶组件（HOC）和 render props 更简洁、直观。
- 
### 3. 更少冗余、语法更简洁

- 无需写类、构造函数、`this.state`、`this.setState`。
- 函数组件代码更精简、更符合 JavaScript 函数式风格。

### 4. 更好地组织逻辑

- 按功能组织（比如一个组件中状态、监听、请求逻辑分块明确），不再按生命周期拆分。
- 避免生命周期方法中掺杂不相关逻辑。

### 5. 社区生态丰富

- 众多第三方库（如 `react-query`, `zustand`, `jotai`, `redux-toolkit`）提供了基于 Hook 的强大能力，成为主流开发模式。

## 缺点

### 1. 使用不当易引发 bug

闭包陷阱、无限渲染、依赖数组问题等是新手高频踩坑点。

例如忘记在 useEffect 依赖数组中添加某个变量。

useCallback、useMemo 等优化不当反而导致性能下降。

### 2. 难以调试和测试

自定义 Hook 内部状态较难单步调试。

测试复杂 Hook（如涉及副作用或异步操作）需要额外 mock、控制时序。

### 3. 抽象过度时难以理解

高度封装的自定义 Hook，容易形成“黑盒”，阅读和维护成本增加。

Hook 调用必须保持顺序，不能放在条件语句中，这对理解调用流程是种挑战。

### 4. 性能优化不够直观

函数组件每次 render 都重新执行函数体，若不合理使用 useMemo / useCallback，可能频繁重新计算、重新创建函数。



## 旧数据问题：

### [useEffect](react/useEffect)/[useCallback](react/useCallback)/[useMemo](react/useMemo) 的依赖项写错或缺失

```
const [count, setCount] = useState(0);

useEffect(() => {
  const timer = setInterval(() => {
    console.log(count); // ❌ 始终打印初始 count 值
  }, 1000);

  return () => clearInterval(timer);
}, []); // ✅ count 没有写在依赖里，闭包引用了旧的值

```

解决方案：
[setState](react/useState)函数形式调用：

```
useEffect(() => {
  const timer = setInterval(() => {
    setCount(prev => {
      console.log(prev); // ✅ 正确引用
      return prev + 1;
    });
  },
   1000);
  return () => clearInterval(timer);
}, []);
```

#### 原理？TODO

### 事件处理函数中引用旧的 state/props（闭包陷阱）

```
const [name, setName] = useState('Alice');

function handleClick() {
  setTimeout(() => {
    console.log(name); // ❌ 始终是点击时的 name 值，不是最新的
  }, 2000);
}
```

解决方案：
用 [ref](react/useRef) 保持同步值：

```
const nameRef = useRef(name);
useEffect(() => {
  nameRef.current = name;
}, [name]);

function handleClick() {
  setTimeout(() => {
    console.log(nameRef.current); // ✅ 始终是最新值
  }, 2000);
}
```


### 自定义 hook 中没有正确依赖更新

```
function useCustom(data) {
  useEffect(() => {
    console.log(data); // ❌ 如果调用处传进来的是变化的，但没有监听 data，就不会更新
  }, []);
}
```

解决方案：

```
useEffect(() => {
  console.log(data);
}, [data]); // ✅ 监听 data
```

### [useCallback](react/useCallback)/[useMemo](react/useMemo) 缓存了闭包，引用了旧变量

```
const [text, setText] = useState('');

const handleSubmit = useCallback(() => {
  console.log(text); // ❌ 始终是创建时的 text
}, []); // 忘记加 [text]
```

解决方案：

```
const handleSubmit = useCallback(() => {
  console.log(text); // ✅ 获取最新 text
}, [text]);
```