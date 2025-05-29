`requestAnimationFrame` 是浏览器专门为高性能动画设计的 API，它会在 **下一帧绘制之前**调用你提供的函数，是替代 `setTimeout` 或 `setInterval` 实现动画的推荐方案。

```
const id = requestAnimationFrame(callback);
cancelAnimationFrame(id); // 取消该帧
```

- `callback` 会被浏览器在 **下一帧渲染前**执行
    
- 浏览器会传入一个高精度时间戳参数：`callback(timestamp)`
    
- 推荐用递归方式持续执行动画


常见使用场景：动画

```
<div id="box" style="width:50px; height:50px; background:red; position:absolute;"></div>
```

```
const box = document.getElementById('box');
let startTime = null;

function move(timestamp) {
  if (!startTime) startTime = timestamp;

  const progress = timestamp - startTime;
  const distance = Math.min(progress / 2, 300); // 300px max

  box.style.transform = `translateX(${distance}px)`;

  if (distance < 300) {
    requestAnimationFrame(move); // 下一帧继续
  }
}

requestAnimationFrame(move);
```

### 为什么用它而不用 `setInterval`？

| 对比项      | `setInterval` | `requestAnimationFrame` |
| -------- | ------------- | ----------------------- |
| 是否同步屏幕刷新 | ❌ 否           | ✅ 是，适配设备刷新率（通常60fps）    |
| 节能优化     | ❌ 仍执行         | ✅ 后台 tab 会暂停            |
| 是否平滑动画   | ❌ 容易跳帧卡顿      | ✅ 平滑，系统节流防止过度渲染         |
| 是否对开发者友好 | 一般            | ✅ 有时间戳 + 自适应            |

在页面 **绘制完成之后再做某些操作**（如动画、滚动、焦点控制）

```
requestAnimationFrame(() => {
  // DOM 已准备好，立即绘制
  inputRef.current?.focus();
});

```

连续两帧判断页面是否真正“绘制完成”

```
requestAnimationFrame(() => {
  requestAnimationFrame(() => {
    console.log('页面应该已经绘制完成了');
  });
});
```

注意：

- 每次执行都要手动调用下一帧：递归模式
    
- 可通过 `cancelAnimationFrame(id)` 取消某一帧（常用于组件卸载、动画打断）
    
- 不推荐用于精确定时逻辑（比如每秒执行一次），这类需求应使用 `setInterval`


- **跟随屏幕刷新率**
    
- **节能优化（后台页不会浪费）**
    
- **适合 DOM 操作、Canvas 绘图、过渡动画等**


### 下一帧渲染前 指的是什么？

浏览器大致每 **16.6ms**（60fps）进行一次屏幕刷新。一次完整的渲染帧可以简化为如下流程：

```
┌─────────────────────────────────────┐
│           一帧渲染流程               │
├─────────────────────────────────────┤
│   JavaScript 执行阶段（任务）       │
│   ↓                                 │
│   requestAnimationFrame 回调执行    │←——（这里！）    
│   ↓                                 │
│   样式计算（Style）                 │
│   ↓                                 │
│   布局（Layout）                    │
│   ↓                                 │
│   绘制（Paint）                     │
│   ↓                                 │
│   合成和显示（Composite + Display）│
└─────────────────────────────────────┘
```

React 的更新流程（特别是函数组件 + hooks）大致如下:

```
render phase:
  - 函数组件执行（返回 JSX）
  - 收集 hooks、effects 等

commit phase:
  - DOM 修改
  - useLayoutEffect 触发
  - 浏览器绘制前（此时 requestAnimationFrame 会运行）
  - useEffect 触发（绘制之后）

```

```
┌─────────────────────┐
│ 组件 render          │
├─────────────────────┤
│ 虚拟 DOM diff        │
├─────────────────────┤
│ commit 更新 DOM      │
├─────────────────────┤
│ useLayoutEffect 调用 │ ← DOM 已更新，绘制前
├─────────────────────┤
│ requestAnimationFrame│ ← 通常插在此阶段
├─────────────────────┤
│ 浏览器开始绘制        │
├─────────────────────┤
│ useEffect 调用       │ ← DOM 已绘制完成
└─────────────────────┘

```