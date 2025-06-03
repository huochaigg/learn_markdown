在 TypeScript 中，`type` 是一种 **类型别名（Type Alias）**，用于给任意合法的类型起一个名字

## 基本语法

```
type ID = number
type Name = string
type User = {
  id: ID
  name: Name
}
```

## 联合类型

```
type Status = 'success' | 'error' | 'loading'

let state: Status = 'success' // 只能是三个字符串之一
```

## 交叉类型

```
type A = { name: string }
type B = { age: number }

type C = A & B // { name: string, age: number }
```

## 类型别名嵌套组合

```
type Point = {
  x: number
  y: number
}

type LabeledPoint = Point & { label: string }
```

## 泛型类型别名

```
type ApiResponse<T> = {
  code: number
  data: T
  message: string
}

const res: ApiResponse<string[]> = {
  code: 200,
  data: ['a', 'b'],
  message: 'OK'
}
```

## 条件类型

```
type IsString<T> = T extends string ? true : false

type A = IsString<'abc'>  // true
type B = IsString<123>    // false
```

## 类型映射

```
type Options = 'bold' | 'italic' | 'underline'

type Flags = {
  [K in Options]: boolean
}

// 等价于：
/*
type Flags = {
  bold: boolean
  italic: boolean
  underline: boolean
}
*/
```

## 类型工具

```
type PartialUser = Partial<{ name: string; age: number }>
type ReadonlyUser = Readonly<{ name: string; age: number }>
type PickName = Pick<{ name: string; age: number }, 'name'>
```

## 描述函数类型

```
type Fn = (x: number, y: number) => number
const add: Fn = (a, b) => a + b
```

## 描述元组

```
type Point = [number, number]
const pos: Point = [10, 20]
```