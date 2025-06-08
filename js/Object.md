
## Object.is

`Object.is()` 是 JavaScript 中用于比较两个值是否**完全相同**的一个方法，语法如下：

```
Object.is(value1, value2)
```

它和 [`===`](js/==和===)（严格相等）很像，但在某些边界情况下更准确

| 比较方式                  | 是否相等    | 说明              |
| --------------------- | ------- | --------------- |
| `Object.is(+0, -0)`   | ❌ false | 区分正零与负零         |
| `+0 === -0`           | ✅ true  | 严格相等不区分正负零      |
| `Object.is(NaN, NaN)` | ✅ true  | 可以正确判断 NaN 相等   |
| `NaN === NaN`         | ❌ false | 严格相等中 NaN 不等于自己 |

和 `===` 的区别

|情况|`===`|`Object.is`|
|---|---|---|
|`NaN`|false|true|
|`+0, -0`|true|false|
|其他基本类型|同 `===`|相同|


## Object.assign