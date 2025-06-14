---
sidebar_position: 1
---

# Promise Categories

## promise.all

> 等待所有 Promise 都完成

```javascript
const promiseAll = (promises) => {
  return new Promise((resolve, reject) => {
    if (!Array.array(promises)) {
      return reject(new TypeError("argument must be an array"));
    }

    let res = [];
    let completed = 0;

    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then((value) => {
          // 將結果存入 res 陣列
          res[index] = value;
          completed++;

          // 如果所有 Promise 都已完成，則返回結果
          if (completed === promises.length) {
            resolve(res);
          }
        })
        .catch(reject); // 如果其中一個 Promise 失敗，則返回失敗的結果
    });
  });
};
```

## promise.allSettled

> 等待所有 Promise 都完成，不論成功或失敗
> 與 promise.all 不同，promise.allSettled 不會因為其中一個 Promise 失敗而 reject

```javascript
const promiseAllSettled = (promises) => {
  return new Promise((resolve) => {
    const res = [];
    let completed = 0;

    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then((val) => {
          res[index] = { status: "fulfilled", val };
        })
        .catch((reason) => {
          res[index] = { status: "rejected", reason };
        })
        .finally(() => {
          completed++;
          if (completed === promises.length) {
            resolve(res);
          }
        });
    });
  });
};
```

# promise.race

> 等待第一個 Promise 完成

```javascript
const promiseRace = (promises) => {};
```

# promise.any

> 等待第一個 Promise 完成，不論成功或失敗

```javascript
const promiseAny = (promises) => {};
```

# promise.resolve

> 返回一個已經完成的 Promise

```javascript
const promiseResolve = (value) => {};
```
