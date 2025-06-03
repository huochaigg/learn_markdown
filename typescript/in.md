TypeScript 中`in` 主要有类型映射，运行时检查对象是否包含某个属性两种用法，in的后面跟一个联合类型

## 类型映射

```
type Keys = 'a' | 'b' | 'c';

type MyType = {
  [K in Keys]: number;
};

// 相当于

type MyType = {
  a: number;
  b: number;
  c: number;
};
```

## 运行时检查对象是否包含某个属性

```
interface Cat {
  meow: () => void;
}

interface Dog {
  bark: () => void;
}

function isCat(pet: Cat | Dog): pet is Cat {
  return 'meow' in pet;
}

```


```
type MyType = {
  [K in Keys]?: number;
};

// 相当于
type MyType = {
  a?: number;
  b?: number;
  c?: number;
};
```


## 注意事项：

### 1.in 的后面是一个联合类型
### 2.复杂结构

```
type T = {
  [K in 'a' | 'b']: K extends 'a' ? string : number;
};
```

### 3. 支持 `?`、`readonly` 等修饰符

```
type T = {
  readonly [K in 'a' | 'b']?: number;
};
```

### 4.也可用 [`as`](typescript/as) 重映射键名

```
type T = {
  [K in 'a' | 'b' as `key_${K}`]: boolean;
};
// => { key_a: boolean; key_b: boolean }
```

### 5.联合类型会被展开

```
type K = 'a' | 'b';
type T = { [P in K]: number }; // 同时生成 a 和 b 键
```
