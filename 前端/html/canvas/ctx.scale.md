```
ctx.scale(1.4, 1.4)
```

#### 参数说明：

- **`x`**：水平方向的缩放比例。
- **`y`**：垂直方向的缩放比例。

- **作用**：
	- **缩放绘图内容**：`ctx.scale` 会改变绘图的比例。例如，`ctx.scale(2, 2)` 会将所有后续绘制的内容放大 2 倍。
	- **影响坐标系**：缩放会改变 Canvas 的坐标系。例如，调用 `ctx.scale(2, 2)` 后，Canvas 的坐标 `(1, 1)` 实际上会映射到原始坐标 `(2, 2)`
- **使用场景**：
    1. **适配高分辨率屏幕**：
	    在高分辨率屏幕（如 Retina 屏幕）上，使用 `ctx.scale` 可以根据设备像素比（`devicePixelRatio`）调整绘图的清晰度，避免模糊。
    2. **缩放内容**：
	    用于动态调整绘图内容的大小，例如放大或缩小图形。
    3. **实现缩放动画**：
	    可以结合 [`requestAnimationFrame`](浏览器/网页相关/requestAnimationFrame) 实现动态缩放效果。

#### 使用例子：

```
const canvas = document.getElementById('myCanvas') as HTMLCanvasElement;
const ctx = canvas.getContext('2d')!;

// 设置 Canvas 的宽高
canvas.width = 200;
canvas.height = 200;

// 缩放之前绘制矩形
ctx.fillStyle = 'blue';
ctx.fillRect(10, 10, 50, 50);

// 应用缩放
ctx.scale(2, 2);

// 缩放之后绘制矩形
ctx.fillStyle = 'red';
ctx.fillRect(10, 10, 50, 50);
```

##### 效果：

- 蓝色矩形是未缩放的，大小为 `50x50`。
- 红色矩形是缩放后的，实际大小为 `100x100`。