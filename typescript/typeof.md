在typescript中，typeof有两种主要用法：

## 值

```
console.log(typeof 'hello world') // string
```

## 类型：

提取某个值的类型，用于声明其他变量或类型:

```
const user = {
  name: "Alice",
  age: 30
};

type User = typeof user;
// 等价于：
/*
type User = {
  name: string;
  age: number;
}
*/

const anotherUser: User = {
  name: "Bob",
  age: 25
};
```

## 问题：

### typeof null 的结果是什么，为什么？

`typeof null` 的结果是 `"object"`。这是一个 JavaScript 的古老错误

### typeof NaN

```
type NaNType = typeof NaN // 'number'
```

NaN：Not a Number，表示非数字

### typeof 是否能正确判断类型？

对于原始类型来说，除了 null 都可以调用typeof显示正确的类型。

```
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol() // 'symbol'
```

但对于引用数据类型，除了函数之外，都会显示"object"。

```
typeof [] // 'object'
typeof {} // 'object'
typeof console.log // 'function'
```

因此采用typeof判断对象数据类型是不合适的，采用instanceof会更好，instanceof的原理是基于原型链的查询，只要处于原型链中，判断永远为true

```
const Person = function() {}
const p1 = new Person()
p1 instanceof Person // true
var str1 = 'hello world'
str1 instanceof String // false
var str2 = new String('hello world')
str2 instanceof String // true
```