在 TypeScript 中，`as` 是 **类型断言（Type Assertion）** 的关键字，用于告诉编译器，你很确定这个值是某个类型，它不会做任何 **类型转换**，只是用来 **辅助编译器理解类型**

```
let str = "123";
let num = str as unknown as number;
```

这 **不是把字符串转换成数字**，只是编译器认为 `num` 是 `number` 类型。但运行时它仍然是 `"123"` 字符串

## 类型断言的常见用法

### 缩小类型范围（类型收窄）

```
function handle(value: string | number) {
  if ((value as string).toUpperCase) {
    console.log((value as string).toUpperCase());
  }
}
```

### 将 DOM 类型断言为具体元素类型

```
const input = document.querySelector('input') as HTMLInputElement;
console.log(input.value);
```

不加 `as`，`querySelector` 只能推断为 `Element | null`，没有 `.value` 属性


### 和 `unknown` 联合使用

```
function parseData(data: string): unknown {
  return JSON.parse(data);
}

const result = parseData('{"name": "Tom"}') as { name: string };
```

## `as const` 是 `as` 的特殊形式

`as const` 是断言为最窄字面量类型 + readonly 的语法糖

```
const arr = [1, 2, 3] as const;
// 类型是 readonly [1, 2, 3]
```

## 注意：

滥用断言引起的问题：

```
const el = document.querySelector('input') as HTMLDivElement; // 错误但编译器不报错，这样不安全
```

解决方法：

```
const el = document.querySelector('input');

if (el instanceof HTMLInputElement) {
  // 现在 el 的类型会自动缩小为 HTMLInputElement
  console.log(el.value);
}

// 或封装成类型方法
function queryInput(selector: string): HTMLInputElement | null {
  const el = document.querySelector(selector);
  return el instanceof HTMLInputElement ? el : null;
}

const input = queryInput('input');
if (input) {
  console.log(input.value);
}
```


## as重映射键名

语法：`as` + `keyof` + 模板字面量类型

```
type RenameKeys<T> = {
  [K in keyof T as `new_${string & K}`]: T[K];
};
```

删除某些键，注意这里[extends](typescript/extends)的用法

```
type Original = {
  id: number;
  name: string;
  password: string;
};

// 只保留非 password 的字段
type WithoutPassword = {
  [K in keyof Original as K extends "password" ? never : K]: Original[K];
};

// 得到：
/*
type WithoutPassword = {
  id: number;
  name: string;
}
*/
```

### 将所有字段变为大写键名

```
type Original = {
  foo: number;
  barBaz: string;
};

type UppercaseKeys = {
  [K in keyof Original as Uppercase<string & K>]: Original[K];
};

// 得到：
/*
type UppercaseKeys = {
  FOO: number;
  BARBAZ: string;
}
*/
```

## 扩展

as 改名技巧表

| 操作      | 示例语法                                                                  | 示例结果 (输入: `{ name: string; age: number }`)                        |
| ------- | --------------------------------------------------------------------- | ----------------------------------------------------------------- |
| 加前缀     | `[K in keyof T as` prefix_${string & K}`]`                            | `{ prefix_name: string; prefix_age: number }`                     |
| 加后缀     | `[K in keyof T as` ${string & K}_suffix`]`                            | `{ name_suffix: string; age_suffix: number }`                     |
| 大写字段名   | `[K in keyof T as Uppercase<string & K>]`                             | `{ NAME: string; AGE: number }`                                   |
| 小写字段名   | `[K in keyof T as Lowercase<string & K>]`                             | `{ name: string; age: number }`（原样，除非有大写）                         |
| 驼峰转下划线名 | `[K in keyof T as K extends string ? SnakeCase<K> : never]` _(需工具类型)_ | `{ name: string; age: number }` → `{ name: string; age: number }` |
| 过滤字段    | `[K in keyof T as K extends 'age' ? never : K]`                       | `{ name: string }`                                                |
| 只保留字符串值 | `[K in keyof T as T[K] extends string ? K : never]`                   | `{ name: string }`（假设 `age` 是 `number`）                           |
| 映射为常量字段 | `[K in keyof T as` ${K & string}_CONST`]: "CONST"`                    | `{ name_CONST: "CONST"; age_CONST: "CONST" }`                     |
