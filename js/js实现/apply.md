```
Function.paototype.myApply = function (ctx, argsArray) {
	ctx = ctx || globalThis;

	const fnSymbol = Symbol();
	ctx[fnSymbol] = this;
	let result;
	if (Array.isArray(argsArray)) {
		result = context[fnSymbol](...argsArray);
	} else {
		result = context[fnSymbol]();
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