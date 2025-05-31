React 是由 Facebook（现 Meta）开发并开源的一个 **用于构建用户界面的 JavaScript 库**，主要用于开发 Web 和原生应用中的视图层。它强调 **组件化、声明式 UI 和单向数据流**，非常适合构建交互复杂的前端项目。

## React 核心特点

### 1. 组件化

React 将 UI 拆分为一个个独立的、可复用的组件，每个组件都拥有自己的状态和逻辑。例如：

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

### 2. JSX 语法

JSX 是 JavaScript 的语法扩展，它允许在 JavaScript 中写类似 HTML 的标签：

```
const element = <h1>Hello, world!</h1>;
```

它本质上是被 Babel 编译为：

```
React.createElement('h1', null, 'Hello, world!');
```

### 3. 虚拟 DOM（Virtual DOM）

React 并不直接操作真实 DOM，而是先构建一棵虚拟 DOM 树，每次状态更新时比较前后的虚拟 DOM 差异（diff），最后只更新有变化的部分到真实 DOM，提升性能。

### 4.单向数据流

### 5. [hooks](react/hooks) (16.8以上版本)

Hooks 是一种在函数组件中使用状态和副作用等特性的方式