### 用Generator写一个async函数
```
function asyncify(generatorFunc) {
  return function (...args) {
	const gen = generatorFunc.apply(this, args);
	console.log('gen:', gen);
	return new Promise((resolve, reject) => {
	  function step(key, arg) {
		let result;
		try {
		  result = gen[key](arg);
		} catch (err) {
		  reject(err);
		  return;
		}

		const { value, done } = result;
		if (done) {
		  resolve(value);
		} else {
		  // value 预期是一个 Promise，等待它完成再继续
		  Promise.resolve(value).then(
			val => step('next', val),
			err => step('throw', err)
		  );
		}
	  }
	  step('next');
	});
  };
}

function fetchSomething() {
  return new Promise(res => setTimeout(() => res('你好你好'), 1000));
}

// 使用 generator + 手写 async
const myAsyncFunc = asyncify(function* () {
  console.log('start');
  const data = yield fetchSomething();
  console.log('got:', data);
  return data;
});

console.log('myAsyncFunc:', myAsyncFunc);

myAsyncFunc().then(result => {
  console.log('final result:', result);
});
```