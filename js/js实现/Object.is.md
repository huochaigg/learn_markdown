```
function is(x, y) {
  if (x === y) {
    // 针对 +0 和 -0 的处理
    return x !== 0 || 1 / x === 1 / y;
  } else {
    // 针对 NaN 的处理
    return x !== x && y !== y;
  }
}
```

