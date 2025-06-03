
TypeScript 中的 `interface` 用于描述对象和类型

## 基础语法

```
interface Person {
  name: string
  age: number
}
```

等价于：

```
const user: Person = {
  name: 'Tom',
  age: 30
}
```


## 可选属性

```
interface Person {
  name: string
  age?: number // 可选属性
}
```

## 只读属性

```
interface Point {
  readonly x: number
  readonly y: number
}
```

赋值后不可修改

```
const p: Point = { x: 1, y: 2 }
// p.x = 10 // ❌ 报错
```

## 函数类型接口

```
interface Greet {
  (name: string): string
}

const sayHello: Greet = name => `Hello, ${name}`
```

## 索引签名

```
interface Dictionary {
  [key: string]: string
}

// 多个类型
interface NumericDictionary {
  [key: string]: number | undefined
}
```

## 接口继承

```
interface Animal {
  name: string
}

interface Dog extends Animal {
  bark(): void
}

// 多继承
interface A { a: number }
interface B { b: number }
interface C extends A, B {
  c: number
}

```

## 描述类结构

```
interface Person {
  name: string
  sayHi(): void
}

class Student implements Person {
  name = 'Jack'
  sayHi() {
    console.log('Hi')
  }
}
```

## 描述构造函数

```
interface Constructor {
  new (name: string): Person
}

function createInstance(Ctor: Constructor, name: string) {
  return new Ctor(name)
}

```

## 泛型类接口

```
interface ApiResponse<T> {
  code: number
  data: T
  message: string
}

const response: ApiResponse<string[]> = {
  code: 200,
  data: ['a', 'b'],
  message: 'ok'
}
```

注意：

```
interface A {
  a: string;
}

interface A {
  a: number;
  b: number;
}

// 这样会报错，接口声明合并冲突
// TypeScript 允许 **同名 `interface` 自动合并（声明合并）**，即多个同名 `interface` 会被合并为一个
// **但！**如果两个合并的接口中有 **同名属性且类型不兼容**，就会报错

```