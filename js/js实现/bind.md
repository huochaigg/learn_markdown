完整版，考虑构造函数的调用

```
Function.prototype.myBind = function (context, ...bindArgs) {
  const fn = this;

  function boundFunction(...callArgs) {
    const isNew = this instanceof boundFunction; // 判断是否用于构造函数
    const finalContext = isNew ? this : context || globalThis;
    return fn.apply(finalContext, [...bindArgs, ...callArgs]);
  }

  // 保持原函数原型，用于构造函数继承
  boundFunction.prototype = Object.create(fn.prototype);
  return boundFunction;
};
```

简单版, 不考虑构造函数

```
Function.prototype.myBind = function(context, ...args) {
	context = context || globalThis;
	context.fn = this; // 保存原函数的引用
	return function(...newArgs) {
		const result = context.fn(...args, ...newArgs); // 执行函数并传入参数
		delete context.fn; // 删除临时函数
		return result; // 返回结果
	};
};
```

