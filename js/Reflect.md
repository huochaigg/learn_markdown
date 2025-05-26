`Reflect` 对象是 ES6（ES2015）新增的一个对象，专门用来 **操作对象的底层行为**，比如 `get`、`set`、`deleteProperty` 等。可以理解为，它把很多以前散落在各种地方的方法（比如 `Object.defineProperty`、属性访问、函数调用等）都统一收拢到了一个地方，让这些操作更加标准化、函数化。

### 基本内容

| 方法名                                | 功能                    | 类似的语法或函数                                                                    |
| ---------------------------------- | --------------------- | --------------------------------------------------------------------------- |
| `Reflect.get`                      | 获取属性值                 | `obj[prop]`                                                                 |
| `Reflect.set`                      | 设置属性值                 | `obj[prop] = value`                                                         |
| `Reflect.has`                      | 判断属性是否存在              | `'prop' in obj`                                                             |
| `Reflect.deleteProperty`           | 删除属性                  | `delete obj[prop]`                                                          |
| `Reflect.defineProperty`           | 定义属性                  | `Object.defineProperty(obj, prop, desc)`                                    |
| `Reflect.getOwnPropertyDescriptor` | 获取属性描述符               | `Object.getOwnPropertyDescriptor(obj, prop)`                                |
| `Reflect.ownKeys`                  | 获取对象自身所有属性（包括 symbol） | `Object.getOwnPropertyNames(obj).concat(Object.getOwnPropertySymbols(obj))` |
| `Reflect.isExtensible`             | 是否可扩展                 | `Object.isExtensible(obj)`                                                  |
| `Reflect.preventExtensions`        | 阻止扩展                  | `Object.preventExtensions(obj)`                                             |
| `Reflect.getPrototypeOf`           | 获取原型                  | `Object.getPrototypeOf(obj)`                                                |
| `Reflect.setPrototypeOf`           | 设置原型                  | `Object.setPrototypeOf(obj, proto)`                                         |
| `Reflect.apply`                    | 调用函数                  | `func.apply(thisArg, args)`                                                 |
| `Reflect.construct`                | 构造实例                  | `new target(...args)`                                                       |

### Reflect.construct 构造实例

```
Reflect.construct(Parent, [name], new.target);` 是什么意思？
```

`new.target` 是 ES6 引入的一个**元属性**，只能在构造函数或 class 中使用。
它表示当前被 `new` 调用的构造函数本身，而不是被调用的父类。
```
function A() {
  console.log(new.target);
}
new A(); // 👉 打印 A
```

```
function Parent(name) {
  this.name = name;
}
function Child(name) {
  return Reflect.construct(Parent, [name], new.target);
}
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;
```
相当于
```
class Parent {
  constructor(name) {
    this.name = name;
  }
}
class Child extends Parent {
  constructor(name) {
    super(name);
  }
}
```


#### Reflect.construct和 Reflect.apply的区别是：
- Reflect.apply把 `Parent` 作为普通函数调用，并指定 `this`
- 不会创建新对象
- 不会设置原型链
- `this` 必须是现成的对象