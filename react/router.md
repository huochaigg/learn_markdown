## 路由原理

- **Hash 模式（`#`）**：通过监听 `hashchange` 事件；
    
- **History 模式（HTML5 History API）**：通过监听 `popstate` 事件 + 调用 `pushState/replaceState` 改变地址栏。


React Router 的理念是 **“声明式路由”**，通过组件嵌套实现路由配置。

```
window.addEventListener('hashchange', () => {
  // location.hash 变更后重新匹配路由
});
```

```
// 内部监听 popstate 事件
window.addEventListener('popstate', () => {
  // 读取 location.pathname，根据配置匹配并更新组件树
});
```

### 1. 路由核心组成

- `<BrowserRouter>` 或 `<HashRouter>`：提供路由上下文；
    
- `<Routes>` 和 `<Route>`：声明路径与组件；
    
- `useNavigate()`：用于编程式跳转；
    
- `useLocation()`：获取当前路径。