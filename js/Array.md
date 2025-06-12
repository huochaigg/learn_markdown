## fill

### 语法

```
arr.fill(value, start, end)
```

|参数|说明|
|---|---|
|`value`|要填充的值|
|`start`|起始索引（包含），默认是 `0`|
|`end`|结束索引（不包含），默认是数组长度|

```
const arr = [1, 2, 3, 4];
arr.fill(0); 
// [0, 0, 0, 0]
```

```
const arr = [1, 2, 3, 4, 5];
arr.fill(9, 1, 4);
// [1, 9, 9, 9, 5]
```

```
// 创建一个长度为5，值都为0的数组
const arr = new Array(5).fill(0);
// [0, 0, 0, 0, 0]
```

#### 注意：引用类型会共享

```
const arr = new Array(3).fill({});
arr[0].name = 'Jack';
console.log(arr);
// [{name: "Jack"}, {name: "Jack"}, {name: "Jack"}]
```

解决方式：

```
const arr = Array.from({ length: 3 }, () => ({}));
arr[0].name = 'Jack';
console.log(arr);
// [{name: "Jack"}, {}, {}]
```