```
Function.paototype.myCall = function (ctx, ...args) {
	ctx = ctx || globalThis;

	const fnSymbol = Symbol();
	ctx[fnSymbol] = this;
	const result = ctx[fnSymbol](...args);
	delete ctx[fnSymbol];
	return result;
}
```