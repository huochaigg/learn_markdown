在 TypeScript 中，`extends` 有三种常见用法，分别出现在 **泛型约束**、**条件类型** 和 **类继承/接口继承** 中。

### 1.  泛型约束中的 extends

用于限制泛型类型的范围。

```
function printLength<T extends { length: number }>(arg: T): void {
  console.log(arg.length);
}

printLength([1, 2, 3]);         // OK，数组有 length
printLength("hello");          // OK，字符串有 length
// printLength(123);           // ❌ Error: number 没有 length 属性

```

### 2.  条件类型中的 extends

用于实现类型的条件判断（类似 `if...else`）：

```
type IsString<T> = T extends string ? true : false;

type A = IsString<"hello">;   // true
type B = IsString<123>;       // false
```

配合 `infer` 可以提取类型的一部分：

```
type ElementType<T> = T extends (infer U)[] ? U : T;

type A = ElementType<number[]>; // number
type B = ElementType<string>;   // string
```

### 3.  类或接口继承中的 extends

用于类继承父类、接口继承接口、类实现接口：

#### 接口继承

```
interface Animal {
  name: string;
}

interface Dog extends Animal {
  bark(): void;
}
```

#### 类继承

```
class Animal {
  name: string = '';
  move() {
    console.log("Moving...");
  }
}

class Dog extends Animal {
  bark() {
    console.log("Woof!");
  }
}
```