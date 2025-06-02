Context 提供了一种在组件树中传递数据的方法，而不必层层通过 props。

通常用来共享：

- 用户认证信息
    
- 主题（theme）
    
- 多语言（i18n）
    
- 跨页面的全局设置等

```
import React from 'react';

const ThemeContext = React.createContext('light'); // 默认值为 'light'
```

```
// 在组件树中提供 context

<ThemeContext.Provider value="dark">
  <App />
</ThemeContext.Provider>

```

```
// 在组件中使用 useContext

import { useContext } from 'react';

function ThemedButton() {
  const theme = useContext(ThemeContext); // 直接读取当前上下文值
  return <button className={theme}>I am styled by {theme} theme</button>;
}
```

## 对比 props 和 useContext

| 方式         | 特点                   |
| ---------- | -------------------- |
| props      | 数据显式传递，粒度控制好，不利于深层组件 |
| useContext | 避免中间层传递，适合全局状态       |
注意：useContext 只会让组件在 `context` 值**变化时重新渲染**，并不会自动引入**状态管理机制**，不等于 Redux/MobX。


## 结合 [useReducer](react/useReducer) 使用,构建简易状态管理
