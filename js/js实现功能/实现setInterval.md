不使用setTimeout的情况下实现setInterval

```
function mySetInterval(callback, interval) {
  let start = Date.now();

  function loop() {
    const now = Date.now();
    const elapsed = now - start;

    if (elapsed >= interval) {
      callback(); // 执行回调函数
      start = Date.now(); // 重置开始时间
    }

    requestAnimationFrame(loop); // 请求下一帧
  }

  loop(); // 启动循环
}

// 使用示例
mySetInterval(() => {
  console.log('Hello, World!');
}, 1000);
```