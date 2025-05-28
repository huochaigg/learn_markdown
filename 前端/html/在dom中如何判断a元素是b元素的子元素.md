### 方法一：`Node.contains()`

```
a.contains(b); // true 表示 b 是 a 的后代节点（包括子元素）
```

<div id="parent">
  <span id="child"></span>
</div>
<div id="parent">
  <span id="child"></span>
</div>
```
<div id="parent">
  <span id="child"></span>
</div>

// js
const a = document.getElementById('parent');
const b = document.getElementById('child');

console.log(a.contains(b)); // true
console.log(b.contains(a)); // false
```

注意：
- `contains()` 会返回 `true` 如果传入自身（即 `a.contains(a)` 是 `true`）；
- 如果只想判断 **“严格子元素”**（不是自身），可以加一层判断：

```
a !== b && a.contains(b);
```

### 方法二：`Node.compareDocumentPosition()`

```
// 判断 b 是 a 的后代节点
Boolean(a.compareDocumentPosition(b) & Node.DOCUMENT_POSITION_CONTAINED_BY); // true

// 判断 a 是 b 的后代节点
Boolean(b.compareDocumentPosition(a) & Node.DOCUMENT_POSITION_CONTAINED_BY); // false
```

### 方法三：遍历父节点查找（不推荐）

```
function isDescendant(parent, child) {
  let node = child.parentNode;
  while (node) {
    if (node === parent) return true;
    node = node.parentNode;
  }
  return false;
}
```
