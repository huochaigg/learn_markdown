## 基本使用

```
import { useQuery } from '@tanstack/react-query'

function MyComponent() {
  const { data, isLoading, error } = useQuery({
    queryKey: ['todos'],
    queryFn: () => fetch('/api/todos').then(res => res.json())
  });

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <ul>
      {data.map(todo => <li key={todo.id}>{todo.title}</li>)}
    </ul>
  );
}
```


## `useQuery` 的参数

| 参数名                    | 类型                 | 说明                     |
| ---------------------- | ------------------ | ---------------------- |
| `queryKey`             | `Array` 或 `string` | 用于标识唯一查询（必须）           |
| `queryFn`              | `Function`         | 数据获取函数，返回 Promise（必须）  |
| `enabled`              | `boolean`          | 是否自动触发请求，默认 `true`     |
| `staleTime`            | `number`           | 数据“过期”时间（毫秒），用于缓存控制    |
| `cacheTime`            | `number`           | 数据保留在缓存中的时间（默认 5 分钟）   |
| `refetchOnWindowFocus` | `boolean`          | 聚焦窗口时是否重新请求（默认 `true`） |
| `retry`                | `number            | boolean 重试次数，默认3次      |
| `select`               | `(data) => any`    | 数据转换函数，用于加工处理返回值       |
| `onSuccess`            | `function`         | 请求成功后的回调               |
| `onError`              | `function`         | 请求失败后的回调               |
| `placeholderData`      | `any               | () => any`             |


## 生命周期说明

| 生命周期事件            | 说明               |
| ----------------- | ---------------- |
| 首次挂载              | 自动触发 `queryFn()` |
| 浏览器窗口重新聚焦         | 触发重新请求（默认开启）     |
| 网络恢复连接            | 自动重新请求           |
| 手动调用 `refetch`    | 手动更新数据           |
| 多个组件共享同一 queryKey | 只请求一次，自动共享数据     |

## 返回值字段

`useQuery()` 返回一个对象，包含以下字段：

| 字段           | 类型                                | 含义                        |
| ------------ | --------------------------------- | ------------------------- |
| `data`       | any                               | 查询返回的数据                   |
| `isLoading`  | boolean                           | 初始加载时为 true               |
| `isFetching` | boolean                           | 任意请求状态下为 true（包括 refetch） |
| `isError`    | boolean                           | 是否出错                      |
| `error`      | Error                             | 错误对象                      |
| `refetch`    | Function                          | 手动重新请求                    |
| `status`     | 'loading' \| 'error' \| 'success' | 状态统一表示                    |

## 缓存

缓存时间：

```
useQuery({
  queryKey: ['posts'],
  queryFn: fetchPosts,
  cacheTime: 1000 * 60 * 10 // 10分钟
})
```

过期时间：

```
useQuery({
  queryKey: ['posts'],
  queryFn: fetchPosts,
  staleTime: 1000 * 60 // 1分钟内不会自动重新请求
})
```

## 进阶

条件请求

```
const userId = getUserId();

useQuery({
  queryKey: ['user', userId],
  queryFn: () => fetchUser(userId),
  enabled: !!userId // userId 存在才发请求
});

```

数据转换（select）

```
useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodos,
  select: (data) => data.filter(todo => !todo.completed)
});
```

占位数据

```
useQuery({
  queryKey: ['posts'],
  queryFn: fetchPosts,
  placeholderData: []
});

```

依赖多个参数（queryKey 多维结构）

```
useQuery({
  queryKey: ['projects', projectId, 'tasks'],
  queryFn: () => fetchTasksByProject(projectId)
})

```