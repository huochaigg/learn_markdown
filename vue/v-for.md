
## 问题：
### v-for和v-if可不可以一起使用？

在 Vue 中，`v-for` 和 `v-if` 是可以**一起使用**的，但需要注意其**使用位置和执行优先级**，否则可能导致逻辑混乱或性能问题。

```
<div v-for="item in list" v-if="item.visible" :key="item.id">
  {{ item.name }}
</div>
```

执行顺序：

- Vue 会先执行 `v-for`，遍历整个 `list`。
    
- 然后对每一个 `item` 再判断 `v-if` 是否为真。
    
- 所以 `v-if` 是在 `v-for` 遍历之后判断的。

- 即使 `item.visible` 为 `false`，也仍然遍历了该项，**有性能开销**。
    
- 容易让逻辑混乱、难以维护。


### 推荐方式：**用 `computed` 或 `v-if` 包裹整个循环**

提前过滤数据

```
<template>
  <div v-for="item in visibleList" :key="item.id">
    {{ item.name }}
  </div>
</template>

<script setup>
const visibleList = computed(() => list.filter(item => item.visible));
</script>
```

用容器包裹 `v-if`

```
<template>
  <template v-if="showList">
    <div v-for="item in list" :key="item.id">
      {{ item.name }}
    </div>
  </template>
</template>
```