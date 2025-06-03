在 TypeScript 中，`keyof` 是一个 **类型操作符**，用于获取某个类型的所有键（key）组成的联合类型。

## 基本用法

```
type Person = {
  name: string
  age: number
}

type PersonKeys = keyof Person // "name" | "age"
```

