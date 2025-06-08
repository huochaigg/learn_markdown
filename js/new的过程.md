### 步骤 1：创建一个空对象（我们称为 `obj`）

```
const obj = {}; // 新对象
```

### 步骤 2：设置原型（链接到构造函数的原型）

```
obj.__proto__ = Person.prototype;
```

这一步确保新对象能访问构造函数原型上的方法。

### 步骤 3：将构造函数内部的 `this` 指向新对象，并执行构造函数


```
const result = Person.call(obj, 'Tom');
```

也就是在新对象的上下文中调用构造函数，给新对象赋值。

### 步骤 4：根据返回值决定最终返回什么


```
if (typeof result === 'object' && result !== null) {
  return result; // 如果构造函数显式返回一个对象，则返回它
}
return obj; // 否则返回创建的新对象
```

### 总结成函数如下（手写 new 实现）：

```
function myNew(Constructor, ...args) {
  const obj = {};
  obj.__proto__ = Constructor.prototype;
  const result = Constructor.apply(obj, args);
  return (typeof result === 'object' && result !== null) ? result : obj;
}
```