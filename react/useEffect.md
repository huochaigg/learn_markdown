- 在 **浏览器完成绘制（paint）之后** 异步执行。
    
- 也就是说，React 渲染完 UI 并且把内容画到屏幕后，才调用 `useEffect` 的回调。
    
- 不阻塞浏览器绘制，适合做数据请求、订阅、事件监听、定时器等“非阻塞”副作用。

`useEffect` → **异步执行，渲染后调用**，不会阻塞浏览器绘制。([对比 useLayoutEffect](/react/useLayoutEffect))


| 需求                  | 推荐使用              |
| ------------------- | ----------------- |
| 发送网络请求，订阅事件，日志打印    | `useEffect`       |
| 读取 DOM 尺寸，手动修改布局或样式 | `useLayoutEffect` |
| 动画启动前调整样式，防止闪烁      | `useLayoutEffect` |

```
import React, { useEffect, useLayoutEffect, useState, useRef } from 'react';

function Demo() {
  const [width, setWidth] = useState(0);
  const divRef = useRef();

  // useLayoutEffect 同步读取和修改 DOM，避免闪烁
  useLayoutEffect(() => {
    const rect = divRef.current.getBoundingClientRect();
    setWidth(rect.width);
    // 这里修改状态，重新渲染前会同步执行，避免绘制不一致
  }, []);

  // useEffect 异步执行，不阻塞绘制
  useEffect(() => {
    console.log('DOM 渲染完成，可以安全访问 DOM');
  }, []);

  return <div ref={divRef} style={{ width: width || 100 }}>宽度：{width}</div>;
}
```