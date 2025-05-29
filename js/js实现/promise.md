```
const PENDING = 'pending';
const FULFILLED = 'fulfilled';
const REJECTED = 'rejected';

class MyPromise {
  constructor(executor) {
    this.status = PENDING;
    this.value = undefined;
    this.reason = undefined;

    this.onFulfilledCallbacks = [];
    this.onRejectedCallbacks = [];

    const resolve = (value) => {
      if (this.status === PENDING) {
        queueMicrotask(() => {
          this.status = FULFILLED;
          this.value = value;
          this.onFulfilledCallbacks.forEach(fn => fn(this.value));
        });
      }
    };

    const reject = (reason) => {
      if (this.status === PENDING) {
        queueMicrotask(() => {
          this.status = REJECTED;
          this.reason = reason;
          this.onRejectedCallbacks.forEach(fn => fn(this.reason));
        });
      }
    };

    try {
      executor(resolve, reject);
    } catch (err) {
      reject(err);
    }
  }

  then(onFulfilled, onRejected) {
    // 保证参数是函数
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : v => v;
    onRejected = typeof onRejected === 'function' ? onRejected : err => { throw err };

    const nextPromise = new MyPromise((resolve, reject) => {
      if (this.status === FULFILLED) {
        queueMicrotask(() => {
          try {
            const x = onFulfilled(this.value);
            resolvePromise(nextPromise, x, resolve, reject);
          } catch (err) {
            reject(err);
          }
        });
      } else if (this.status === REJECTED) {
        queueMicrotask(() => {
          try {
            const x = onRejected(this.reason);
            resolvePromise(nextPromise, x, resolve, reject);
          } catch (err) {
            reject(err);
          }
        });
      } else if (this.status === PENDING) {
        this.onFulfilledCallbacks.push((value) => {
          try {
            const x = onFulfilled(value);
            resolvePromise(nextPromise, x, resolve, reject);
          } catch (err) {
            reject(err);
          }
        });

        this.onRejectedCallbacks.push((reason) => {
          try {
            const x = onRejected(reason);
            resolvePromise(nextPromise, x, resolve, reject);
          } catch (err) {
            reject(err);
          }
        });
      }
    });

    return nextPromise;
  }

  catch(onRejected) {
    return this.then(null, onRejected);
  }

  finally(callback) {
    return this.then(
      value => MyPromise.resolve(callback()).then(() => value),
      reason => MyPromise.resolve(callback()).then(() => { throw reason })
    );
  }

  static resolve(value) {
    if (value instanceof MyPromise) {
      return value;
    }
    return new MyPromise(resolve => resolve(value));
  }

  static reject(reason) {
    return new MyPromise((_, reject) => reject(reason));
  }

  static all(promises) {
    return new MyPromise((resolve, reject) => {
      const results = [];
      let count = 0;
      const len = promises.length;

      if (len === 0) return resolve([]);

      promises.forEach((p, i) => {
        MyPromise.resolve(p).then(
          value => {
            results[i] = value;
            count++;
            if (count === len) {
              resolve(results);
            }
          },
          err => reject(err)
        );
      });
    });
  }

  static race(promises) {
    return new MyPromise((resolve, reject) => {
      promises.forEach(p => {
        MyPromise.resolve(p).then(resolve, reject);
      });
    });
  }
}

// 处理 then 里面返回值的解析逻辑（核心）
function resolvePromise(promise, x, resolve, reject) {
  if (promise === x) {
    return reject(new TypeError('Chaining cycle detected'));
  }

  if (x && (typeof x === 'object' || typeof x === 'function')) {
    let called = false;
    try {
      const then = x.then;
      if (typeof then === 'function') {
        then.call(
          x,
          y => {
            if (called) return;
            called = true;
            resolvePromise(promise, y, resolve, reject);
          },
          r => {
            if (called) return;
            called = true;
            reject(r);
          }
        );
      } else {
        if (called) return;
        called = true;
        resolve(x);
      }
    } catch (err) {
      if (called) return;
      called = true;
      reject(err);
    }
  } else {
    resolve(x);
  }
}

```