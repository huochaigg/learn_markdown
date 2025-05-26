
| 工具类型                       | 功能描述                                  |
| -------------------------- | ------------------------------------- |
| `Partial<T>`               | 将类型 `T` 的所有属性变成可选                     |
| `Required<T>`              | 将类型 `T` 的所有属性变成必填                     |
| `Readonly<T>`              | 将类型 `T` 的所有属性变为只读                     |
| `Pick<T, K>`               | 从类型 `T` 中挑选属性 `K` 组成新类型               |
| `Omit<T, K>`               | 从类型 `T` 中排除属性 `K` 组成新类型               |
| `Record<K, T>`             | 构造一个对象类型，键为 `K`，值为 `T`                |
| `Exclude<T, U>`            | 从类型 `T` 中排除可以赋值给类型 `U` 的部分            |
| `Extract<T, U>`            | 提取 `T` 中可以赋值给 `U` 的部分                 |
| `NonNullable<T>`           | 移除 `T` 中的 `null` 和 `undefined`        |
| `ReturnType<T>`            | 获取函数类型 `T` 的返回值类型                     |
| `Parameters<T>`            | 获取函数类型 `T` 的参数类型组成的元组类型               |
| `ConstructorParameters<T>` | 获取构造函数的参数类型元组                         |
| `InstanceType<T>`          | 获取构造函数的实例类型                           |
| `ThisType<T>`              | 设置对象字面量的 `this` 类型（用于上下文绑定）           |
| `Awaited<T>`               | 获取 `Promise` 类型的解析结果（适用于 async/await） |
```
interface User {
  id: number;
  name: string;
  age?: number;
}

// Partial: 所有属性变为可选
type UserPartial = Partial<User>;
// { id?: number; name?: string; age?: number }

// Required: 所有属性变为必选
type UserRequired = Required<User>;
// { id: number; name: string; age: number }

// Pick: 只保留部分属性
type UserName = Pick<User, 'name'>;
// { name: string }

// Omit: 排除某些属性
type UserWithoutAge = Omit<User, 'age'>;
// { id: number; name: string }

// Record: 构造键值对象类型
type RoleMap = Record<'admin' | 'user', User>;
// { admin: User; user: User }

// Exclude: 排除类型
type T = Exclude<'a' | 'b' | 'c', 'a'>;
// "b" | "c"

// Extract: 提取公共部分
type T2 = Extract<'a' | 'b' | 'c', 'a' | 'd'>;
// "a"

// NonNullable: 移除 null 和 undefined
type T3 = NonNullable<string | null | undefined>;
// string

// ReturnType: 获取函数返回值类型
function getUser() {
  return { id: 1, name: 'Tom' };
}
type GetUserReturn = ReturnType<typeof getUser>;
// { id: number; name: string }

// Parameters: 获取函数参数类型
type Params = Parameters<typeof getUser>;
// []

// ThisType: 设置 this 上下文类型（多用于辅助对象方法）

```