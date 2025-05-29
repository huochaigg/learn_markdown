

- 在 **所有 DOM 变更完成后，浏览器绘制之前** 同步执行。
	
- 会阻塞浏览器绘制，React 会先执行 `useLayoutEffect` 中的代码，再进行绘制。
	
- 适合需要同步测量 DOM 布局、调整 DOM 样式的副作用，避免浏览器绘制“闪烁”或布局跳动。


`useLayoutEffect` → **同步执行，渲染前调用**，阻塞浏览器绘制。([对比 useEffect](/react/useEffect))


[requestAnimationFrame](/浏览器/网页相关/requestAnimationFrame)可以在useLayoutEffect中使用来做dom操作