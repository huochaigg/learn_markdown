```
ctx.save();
```

- **作用**：将当前的绘图上下文状态（包括变换矩阵、裁剪区域、样式属性等）保存到一个栈中。
- **使用场景**：
    - 当你需要临时更改绘图上下文的某些属性（如颜色、字体、变换等），但希望在绘制完成后恢复到之前的状态时使用。
    - 通过保存状态，可以避免手动重置属性，简化代码。