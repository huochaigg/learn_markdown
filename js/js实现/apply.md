```
Function.paototype.myApply = function (ctx, argsArray) {
	ctx = ctx || globalThis;

	const fnSymbol = Symbol();
	ctx[fnSymbol] = this;
	let result;
	if (argsArray == null) { // 可能是null或undefined
	  result = context[fnSymbol]();
	} else if (typeof argsArray[Symbol.iterator] === 'function') { // 判断argsArray是否是一个“可迭代对象”（可以被 `for...of` 或展开运算符 `...` 遍历）
	  result = context[fnSymbol](...argsArray);
	} else {
	  throw new TypeError('CreateListFromArrayLike called on non-object');
	}
	delete ctx[fnSymbol];
	return result;
}
```

简单版

```
Function.prototype.myApply = function(context, args) {
	context = context || globalThis;
	context.fn = this;
	const result = context.fn(...args);
	delete context.fn;
	return result;
};
```