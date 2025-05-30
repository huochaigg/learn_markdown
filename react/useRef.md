

## 为什么 `ref` 能解决“拿不到最新数据”的问题？

`useRef` 的关键特性：

- `ref.current` 是一个**可变容器**，可以被修改不会引发组件重新渲染。
- 每次渲染时，`ref` 本身是固定的，但其 `.current` 是共享的、指向最新值。

### 一般来说不需要将ref 加入依赖，因为它永远是稳定引用

可以封装一个 `useLatest()` hook，解决拿不到数据问题
或者useStateRef, 我比较喜欢用[useStateRef](react/hooks实现/useStateRef)

```
function useLatest<T>(value: T) {
  const ref = useRef(value);
  useEffect(() => {
    ref.current = value;
  }, [value]);
  return ref;
}


// 使用

const [count, setCount] = useState(0);
const latestCount = useLatest(count);

useEffect(() => {
    const interval = setInterval(() => {
      console.log('Count:', latestCount.current); // ✅ 永远是最新的 count
    }, 1000);

    return () => clearInterval(interval);
}, []);

// ......
```


#### 手写简单版的useRef

```
let refValue: any; // 模拟全局存储

function fakeUseRef<T>(initialValue: T): { current: T } {
  if (refValue === undefined) {
    refValue = { current: initialValue };
  }
  return refValue;
}
```

实际比较复杂，大致是这个意思吧.....

## useRef、ref、[forwardsRef](react/forwardRef) 的区别

### useRef

- `useRef` 返回的是一个 `{ current: ... }` 的对象。
- 改变 `.current` 不会引起组件重新渲染。
- 主要用来：
    - 保存 DOM 节点引用
    - 保存一个跨渲染周期保持不变的可变值（比如保存定时器 id、缓存旧值等）

### ref

用于绑定到 React 元素上的属性（JSX 中的 `ref`）

```
<input ref={myRef} />
```

- 这是 React 的一个特殊属性，用于将某个 DOM 节点或组件实例“引用”起来。
- 在类组件中，会引用组件实例；在函数组件中，通常搭配 `useRef` 来获取 DOM 节点。
- 可以是：
    - 函数：`ref={el => { ... }}`
    - 对象：`ref={myRef}`（常见于 `useRef`）

### [forwardRef](react/forwardRef)

高阶函数，让**函数组件**也能接收 `ref` 引用

```
import React, { forwardRef } from 'react';

const MyInput = forwardRef((props, ref) => {
  return <input {...props} ref={ref} />;
});

```

```
const inputRef = useRef();
<MyInput ref={inputRef} />
```

疑问：
[为什么不能直接传 `ref` 给子组件？](react/forwardRef)