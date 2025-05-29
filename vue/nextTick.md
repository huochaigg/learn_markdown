`vue.nextTick()` 是 Vue.js 提供的一个异步方法，用于在 **DOM 更新完成之后** 执行一段代码

在你更改数据后，Vue 并不会立刻更新 DOM，而是将更新过程放到一个队列中，并在下一个“tick”中一起执行。这种策略称为 **异步 DOM 更新机制**，是为了提高性能。

如果你想在数据变化后立刻获取最新的 DOM 状态，比如进行 DOM 操作、获取 DOM 高度等，就需要用 `Vue.nextTick()`。

```
<template>
  <div ref="box">{{ message }}</div>
</template>

<script setup>
import { ref, nextTick } from 'vue'

const message = ref('Hello')

function updateMessage() {
  message.value = 'World'

  // 立即读取 DOM 还是旧的内容
  console.log(document.querySelector('div').textContent) // "Hello"

  // DOM 更新后执行
  nextTick(() => {
    console.log(document.querySelector('div').textContent) // "World"
  })
}
</script>
```


注意：

- `nextTick()` 返回一个 Promise，可以使用 `.then()` 或 `await`。
- 通常在你需要访问更新后的 DOM 时使用，否则不需要手动调用。

promise写法：

```
import { nextTick, ref } from 'vue'

const message = ref('Hello')

function update() {
  message.value = 'World'

  nextTick().then(() => {
    // DOM 已更新
    const el = document.querySelector('div')
    console.log('使用 Promise:', el.textContent) // "World"
  })
}
```


await写法：

```
import { nextTick, ref } from 'vue'

const message = ref('Hello')

async function update() {
  message.value = 'World'

  await nextTick()
  const el = document.querySelector('div')
  console.log('使用 await:', el.textContent) // "World"
}
```


TODO(继续完善)
如何利用了事件循环机制？
如果多次赋值，nextTick做了什么？