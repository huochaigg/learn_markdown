## curryAdd

```
const curryAdd = (...initialArgs) => {
	const args = [...initialArgs];
	
	const curried = (...newArgs) => {
		args.push(...newArgs);
		return curried;
	};
		
	curried.toString = () => args.reduce((sum, n) => sum + n, 0);
	curried.valueOf = curried.toString;
	return curried;
};

// ✅ 使用示例：
console.log(+curryAdd(1)(2, 3)(4, 5)); // 输出 15（自动触发 toString 或 valueOf）
```


```
function curry(fn) {
	const curried = (...args) => {
		const next = (...nextArgs) => {
			if (nextArgs.length === 0) {
				// 如果没有更多参数传递，执行函数并返回结果
				return fn(...args);
			}
			// 否则，递归调用 curried，累积参数
			return curried(...args, ...nextArgs);
		};

		// 如果直接调用，则返回结果
		next.valueOf = () => {
			console.log('valueOf')
			return fn(...args)
		};
		next.toString = () => {
			console.log('toString')
			return fn(...args);
		}

		return next;
	};
	return curried();
}

function sum(...args) {
	return args.reduce((acc, curr) => acc + curr, 0); // 默认值为0
}

const curriedSum = curry(sum);
```