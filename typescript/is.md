在 TypeScript 中，`is` 关键字用于 **类型谓词（Type Predicate）**，通常用于自定义类型保护函数，使 TypeScript 能在条件判断中自动缩小变量的类型。

示例：

```
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

function handle(val: unknown) {
  if (isString(val)) {
    // 这里 val 自动变成 string 类型
    console.log(val.toUpperCase());
  } else {
    console.log('不是字符串');
  }
}

```

### 注意：
#### is 只能用于返回值
#### is 不能用于多个参数

举例：

```
function isStringAndNumber(x: unknown, y: unknown): x is string, y is number {
  // ❌ 错误语法，不允许多个 is 谓词
  return typeof x === 'string' && typeof y === 'number';
}

// 正确替代方法
function isString(x: unknown): x is string {
  return typeof x === 'string';
}

function isNumber(y: unknown): y is number {
  return typeof y === 'number';
}

// 在调用处组合使用
function example(x: unknown, y: unknown) {
  if (isString(x) && isNumber(y)) {
    // x: string, y: number ✅ 类型已缩小
    console.log(x.toUpperCase(), y.toFixed(2));
  }
}

```
