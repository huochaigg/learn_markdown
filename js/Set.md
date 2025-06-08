
数组去重+排序

```
const arr = [3, 1, 2, 3, 2];
const result = [...new Set(arr)].sort((a, b) => a - b);
```

找出两个数组的交集：

```
const arr1 = [1, 2, 3];
const arr2 = [2, 3, 4];
const set2 = new Set(arr2);
const intersection = arr1.filter(x => set2.has(x)); // [2, 3]
```

差集：

```
const difference = new Set([...arr1].filter(x => !set2.has(x)));
```

快速查重

```
function hasDuplicate(arr) {
  return new Set(arr).size !== arr.length;
}
```