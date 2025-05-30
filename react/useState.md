

## 如果需要修改深层state，有哪些方法

在 React 中，如果你的 `state` 结构像这样层级很深：

```
state = {
  a: {
    b: {
      c: {
        d: 123,
        other: 'value'
      }
    }
  }
}
```

你想 **只更新 `c` 里的部分字段（如 `d`），又不想破坏其它结构（如 `other`）**，可以使用以下方法：

####  使用结构展开（推荐，最通用）

```
setState(prev => ({
  ...prev,
  a: {
    ...prev.a,
    b: {
      ...prev.a.b,
      c: {
        ...prev.a.b.c,
        d: 456, // 只更新 d
      }
    }
  }
}));

```

#### 使用 Immer（推荐用于深层更新）

```
npm install immer
```

```
import produce from 'immer';

setState(prev =>
  produce(prev, draft => {
    draft.a.b.c.d = 456; // 像操作 mutable 对象一样写法
  })
);
```

#### 将深层 state 拆成更细的粒度（结构优化）

```
const [c, setC] = useState({ d: 123, other: 'value' });

setC(prev => ({ ...prev, d: 456 }));
```

#### 使用 Lodash 的 `_.set`（不推荐直接修改，需配合深拷贝）

```
import _ from 'lodash';

setState(prev => {
  const newState = _.cloneDeep(prev);
  _.set(newState, 'a.b.c.d', 456);
  return newState;
});
```

#### 使用辅助函数封装展开逻辑（用于统一管理）

```
function updateDeep(prev, path, value) {
  const keys = path.split('.');
  const lastKey = keys.pop();
  const newObj = { ...prev };
  let current = newObj;
  for (const key of keys) {
    current[key] = { ...current[key] };
    current = current[key];
  }
  current[lastKey] = value;
  return newObj;
}

// 使用
setState(prev => updateDeep(prev, 'a.b.c.d', 456));
```

## setState 的函数写法

`setState` 的**函数写法（functional update）**是 React 中一种重要机制，用于在状态更新依赖前一个状态值时，确保拿到“**最新的 state**”，以避免闭包问题或并发模式下的错误更新。

语法：

```
setState(prevState => {
  return newState;
});

setCount(prev => prev + 1);
```

例如：

```
const [value, setValue] = useState(0);

useEffect(() => {
  const timer = setInterval(() => {
    setValue(prev => prev + 1); // ✅ 总是基于最新值更新
  }, 1000);
  return () => clearInterval(timer);
}, []);


useEffect(() => {
	const timer = setInterval(() => {
	    setCount(count + 1); // ❌ count 是闭包里的旧值，连续点击容易丢更新
	}, 1000);
	return () => clearInterval(timer);
})

```


### 注意：

连续调用 `setState` 叠加更新时必须使用函数写法。
异步函数、延时函数引用旧 state时、在 handler 中引用最新状态时推荐使用函数写法。

#### 举例：

错误示例：闭包导致拿到旧状态

```
function Counter() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setTimeout(() => {
      // ❌ 这里的 count 永远是 handler 被创建时的旧值
      alert(`Count is: ${count}`);
    }, 1000);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(c => c + 1)}>+1</button>
      <button onClick={handleClick}>Show after 1s</button>
    </div>
  );
}
```

正确示例1：使用 `ref` 获取最新状态

```
function Counter() {
  const [count, setCount] = useState(0);
  const countRef = useRef(count);

  useEffect(() => {
    countRef.current = count; // 每次 count 变都更新 ref
  }, [count]);

  const handleClick = () => {
    setTimeout(() => {
      alert(`Count is: ${countRef.current}`); // ✅ 始终是最新 count
    }, 1000);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(c => c + 1)}>+1</button>
      <button onClick={handleClick}>Show after 1s</button>
    </div>
  );
}
```

正确写法（使用函数式更新，避免旧值）

```
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const handleDelayedIncrement = () => {
    setTimeout(() => {
      // ✅ 使用函数式写法，不管外层闭包捕获的是什么，都能拿到最新值
      setCount(prev => prev + 1);
    }, 1000);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleDelayedIncrement}>延迟 +1（1秒后）</button>
    </div>
  );
}

export default Counter;

```