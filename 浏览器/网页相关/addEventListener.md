
### 基本写法

```
element.addEventListener(type, listener, options);
```

| 参数         | 类型                   | 说明                                            |
| ---------- | -------------------- | --------------------------------------------- |
| `type`     | `string`             | 要监听的事件类型，如 `'click'`、`'keydown'`、`'scroll'` 等 |
| `listener` | `function`           | 事件触发时执行的回调函数，接受一个事件对象参数                       |
| `options`  | `boolean` 或 `object` | 控制监听器的行为（可选）                                  |
#### 第三个参数
##### 为boolean时

- **capture = true**：往下传 → 捕获阶段
- **capture = false**：往上传 → 冒泡阶段
- 默认为false
#### options

**布尔值**（传统写法）：

```
element.addEventListener('click', handler, false); // 冒泡阶段监听
```

**对象写法（推荐）**：

```
element.addEventListener('click', handler, {
  capture: false,     // 是否在捕获阶段触发
  once: true,         // 只执行一次，自动移除
  passive: true       // 表示不会调用 event.preventDefault()，有助于性能
});
```


事件流分为三个阶段：

1. 捕获阶段（从根到目标元素）
    
2. 目标阶段（在目标元素上）
    
3. 冒泡阶段（从目标回到根）

```
document.body.addEventListener('click', e => {
  console.log('冒泡阶段');
}, false);

document.body.addEventListener('click', e => {
  console.log('捕获阶段');
}, true);
```


```
const button = document.querySelector('#myButton');

function handleClick(event) {
  console.log('按钮被点击', event);
}

button.addEventListener('click', handleClick);
```

```
// passive 用于提升滚动性能

window.addEventListener('scroll', () => {
  // 不会调用 preventDefault()
}, { passive: true });
```


#### 移除监听器：`removeEventListener`

```
function handler(event) {
  console.log('点击');
}

button.addEventListener('click', handler);
// 之后某处取消监听
button.removeEventListener('click', handler);
```

#### 多次绑定不会覆盖旧监听器，会新增一个

```
button.addEventListener('click', () => console.log(1));
button.addEventListener('click', () => console.log(2));
```

```
document.querySelector('a').addEventListener('click', function (e) {
  e.preventDefault(); // 阻止跳转
});
```


## 支持的事件类型（部分）

- 鼠标事件：`click`、`dblclick`、`mousedown`、`mouseup`、`mousemove`
    
- 键盘事件：`keydown`、`keyup`、`keypress`
    
- 表单事件：`submit`、`change`、`input`、`focus`、`blur`
    
- 触摸事件：`touchstart`、`touchend`、`touchmove`
    
- 其他：`load`、`resize`、`scroll`、`error`、`contextmenu`、`drag`