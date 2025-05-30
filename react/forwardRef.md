
## 为什么不能直接传 `ref` 给子组件？

```
function Child() {
  return <div>child</div>;
}

function Parent() {
  const ref = useRef();

  return <Child ref={ref} />; // ⚠️ 无效！ref 不会传递给 <div>
}
```

`ref` 没有指向 `<div>`，而是 `null`

原因：**React 中 `ref` 是保留属性，不能直接作为 props 传递**

`ref` 只能作用于：

- DOM 元素：`<div ref={...} />` → ✅ 会被赋值为真实 DOM
- 类组件：`<MyClassComponent ref={...} />` → ✅ 会拿到类实例
- **函数组件：默认不支持 ref** ❌

解法：使用 `forwardRef` 来告诉 React **“这个函数组件支持 ref”**