`data-` 是 HTML5 引入的一种标准自定义属性语法

### 基本语法

```
<div data-属性名="属性值"></div>
```


例子：
```
<div id="box" data-user-id="123" data-role="admin"></div>
```

```
const box = document.getElementById("box");
console.log(box.dataset.userId); // "123"
console.log(box.dataset.role);   // "admin"
```

老方式（不推荐，兼容用）：

```
box.getAttribute("data-user-id"); // "123"
```

删除：

```
delete box.dataset.userId;
```

`data-*` 属性和 `dataset` 在现代浏览器中都有很好兼容性（IE10+），如果需要支持 IE9 及以下，可以退回使用 `getAttribute()` 方式访问。