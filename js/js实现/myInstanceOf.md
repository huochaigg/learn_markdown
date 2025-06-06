```
function myInstanceof(obj, Constructor) {
  if (typeof Constructor !== 'function' || Constructor === null) {
    throw new TypeError('Right-hand side of instanceof must be a function');
  }

  let proto = Object.getPrototypeOf(obj); // 获取 obj 的原型
  const prototype = Constructor.prototype; // 获取构造函数的 prototype

  while (proto !== null) {
    if (proto === prototype) return true;
    proto = Object.getPrototypeOf(proto); // 向上查找原型链
  }

  return false;
}

```

```
function Person() {}
const p = new Person();

console.log(myInstanceof(p, Person));     // true
console.log(myInstanceof(p, Object));     // true
console.log(myInstanceof(p, Array));      // false
```

补充：

- `myInstanceof(null, X)` 会返回 `false`，不会抛错（`null` 没有原型）。
    
- `myInstanceof(1, Number)` 结果是 `false`，因为 `1` 是原始值，不是对象。和原生行为一致。
    
- `myInstanceof(new Number(1), Number)` 是 `true`。