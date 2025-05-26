# Promise
Promise是 JavaScript 中用于处理异步操作的一种对象。它代表一个异步操作的最终完成（fulfilled）或失败（rejected），以及异步操作完成后的值或失败的原因。
## Promise三种状态
1.pending（待定）：初始状态，既没有被解决（fulfilled），也没有被拒绝（rejected）。
2.fulfilled（已解决）：操作成功完成，Promise 得到一个结果值。
3.rejected（已拒绝）：操作失败，Promise 得到一个拒绝的原因。

状态一旦从 pending 转为 fulfilled 或 rejected，就不能再改变。
## Promise 的作用
1.解决回调地狱问题：

在传统的异步编程中，嵌套的回调函数会导致代码难以阅读和维护（即回调地狱）。
Promise 通过链式调用（then 方法）使代码更加清晰。

2.统一异步操作的处理方式：

无论是异步请求、定时器还是文件操作，Promise 提供了统一的接口来处理异步操作。

3.支持链式调用：

Promise 的 then 方法返回一个新的 Promise，可以通过链式调用处理多个异步操作。

4.错误捕获：

Promise 提供了 catch 方法，用于捕获异步操作中的错误，避免嵌套的 try...catch。
## 方法
***resolve 和 reject：***
改变 Promise 的状态，并执行对应的回调函数。

***then 方法：***
根据当前状态执行回调函数。
如果状态是 pending，将回调函数存储起来，等待状态改变后执行。

***catch 方法：***
只处理 Promise 的失败情况，内部调用 then 方法。

***finally 方法：***
无论 Promise 成功还是失败，都会执行 onFinally 回调。
不改变 Promise 的状态或结果。

## 手写Promise
```
class MyPromise {
  constructor(executor) {
    this.state = 'pending'; // 初始状态
    this.value = undefined; // 成功的值
    this.reason = undefined; // 失败的原因
    this.onResolvedCallbacks = []; // 存储成功的回调
    this.onRejectedCallbacks = []; // 存储失败的回调

    const resolve = (value) => {
      if (this.state === 'pending') {
        this.state = 'fulfilled'; // 状态变为 fulfilled
        this.value = value; // 保存成功的值
        this.onResolvedCallbacks.forEach((callback) => callback()); // 执行所有成功回调
      }
    };

    const reject = (reason) => {
      if (this.state === 'pending') {
        this.state = 'rejected'; // 状态变为 rejected
        this.reason = reason; // 保存失败的原因
        this.onRejectedCallbacks.forEach((callback) => callback()); // 执行所有失败回调
      }
    };

    try {
      executor(resolve, reject); // 执行传入的函数
    } catch (error) {
      reject(error); // 捕获异常并调用 reject
    }
  }

  then(onFulfilled, onRejected) {
    console.log('then', this.state)

    // 返回一个新的 Promise 实现链式调用
    return new MyPromise((resolve, reject) => {
      if (this.state === 'fulfilled') {
        try {
          const result = onFulfilled(this.value); // 执行成功回调
          resolve(result); // 将结果传递给下一个 Promise
        } catch (error) {
          reject(error); // 捕获异常并传递给下一个 Promise
        }
      } else if (this.state === 'rejected') {
        try {
          const result = onRejected(this.reason); // 执行失败回调
          resolve(result); // 将结果传递给下一个 Promise
        } catch (error) {
          reject(error); // 捕获异常并传递给下一个 Promise
        }
      } else if (this.state === 'pending') {
        // 如果状态是 pending，将回调存储起来
        this.onResolvedCallbacks.push(() => {
          try {
            const result = onFulfilled(this.value);
            resolve(result);
          } catch (error) {
            reject(error);
          }
        });
        this.onRejectedCallbacks.push(() => {
          try {
            const result = onRejected(this.reason);
            resolve(result);
          } catch (error) {
            reject(error);
          }
        });
      }
    });
  }

  catch(onRejected) {
    return this.then(null, onRejected); // 只处理失败的情况
  }

  finally() {
    return this.then(
      (value) => {
        // 成功时执行的回调
        return value; // 返回成功的值
      },
      (reason) => {
        // 失败时执行的回调
        throw reason; // 抛出失败的原因
      }
    );
  }

  // 静态方法：Promise.all
  static all(promises) {
    return new MyPromise((resolve, reject) => {
      const results = [];
      let count = 0;

      promises.forEach((promise, index) => {
        promise.then((value) => {
          results[index] = value; // 保存每个 Promise 的结果
          count++;
          if (count === promises.length) {
            resolve(results); // 所有 Promise 成功后 resolve
          }
        }, reject); // 如果有一个 Promise 失败，直接 reject
      });
    });
  }

  // 静态方法：Promise.race
  static race(promises) {
    return new MyPromise((resolve, reject) => {
      promises.forEach((promise) => {
        promise.then(resolve, reject); // 只要有一个 Promise 完成，就 resolve 或 reject
      });
    });
  }
}


const promise = new MyPromise((resolve, reject) => {
  setTimeout(() => {
    console.log('log 成功')
    resolve('成功');
  }, 3000);
});

promise
  .then((result) => {
    console.log('Promise:', result);
    return 'hello'
  })

// 如果需要在then中执行 if (this.state === 'fulfilled'), 可以将.then的调用放在异步操作完成之后：
setTimeout(() => {
  promise
    .then((result => {
      console.log('Promise:', result); // 这里的 result 是上一个 then 的返回值
      return '1成功'; // 返回一个普通值
    }))

  promise
    .then((result => {
      console.log('Promise:', result); // 这里的 result 是上一个 then 的返回值
      return '2成功'; // 返回一个普通值
    }))
}, 5000)
```

