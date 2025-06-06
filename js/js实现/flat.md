```
Array.prototype.myFlat = function(depth = 1) {
    // 如果深度为 0，直接返回当前数组的副本
    if (depth < 1) {
        return [...this];
    }

    // 递归扁平化数组
    const flatten = (arr, d) => {
        let result = [];
        for (const item of arr) {
            // 如果元素是数组并且深度大于 0，则递归扁平化
            if (Array.isArray(item) && d > 0) {
                result.push(...flatten(item, d - 1));
            } else {
                result.push(item);
            }
        }
        return result;
    };

    return flatten(this, depth);
};

// 示例用法
const arr = [1, [2, [3, [4, 5]]], 6];
console.log(arr.myFlat());       // 输出: [1, 2, [3, [4, 5]], 6]
console.log(arr.myFlat(1));      // 输出: [1, 2, [3, [4, 5]], 6]
console.log(arr.myFlat(2));      // 输出: [1, 2, 3, [4, 5], 6]
console.log(arr.myFlat(3));      // 输出: [1, 2, 3, 4, 5, 6]
console.log(arr.myFlat(0));      // 输出: [1, [2, [3, [4, 5]]], 6]
```