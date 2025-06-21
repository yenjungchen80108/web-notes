---
sidebar_position: 4
---

# Debounce & Throttle

debounce and throttle are used to limit the rate at which a function can fire.

## Debounce

Debounce is used to limit the rate at which a function can fire. It will wait for a certain amount of time to pass before calling the function again.

等最後一次操作結束後 n 毫秒才觸發。期間內再觸發會重置計時器。

```javascript
let counter = 0;

const fetchData = () => {
  console.log("fetching data", counter++);
};

const debounce = (func, delay) => {
  let timer;

  return function () {
    // 保存 this 的值
    let context = this;
    // 保存 arguments 的值
    let args = arguments;

    // 如果 timer 存在，則清除 timer
    if (timer) clearTimeout(timer);
    // 設置 timer
    timer = setTimeout(() => fetchData.apply(context, args), delay);
  };
};

// 限制 fetchData 函數執行頻率，在 500ms 內只執行一次
const debouncedFetchData = debounce(fetchData, 500);

debouncedFetchData();
debouncedFetchData();
debouncedFetchData();
```

## Throttle

Throttle is used to limit the rate at which a function can fire. It will call the function immediately and then wait for a certain amount of time to pass before calling the function again.

每隔 n 毫秒最多執行一次，即使事件連續觸發也會被限制頻率。

```javascript
const fetchData = () => {
  console.log("fetching data", counter++);
};

const throttle = (func, limit) => {
  let flag = true;

  return function () {
    let context = this;

    let args = arguments;

    if (flag) {
      // 執行函數
      // highlight-start
      func.apply(context, args);
      // highlight-end
      // 設置 flag 為 false
      flag = false;
      // 在設定的時間間隔內，不會再執行函數
      setTimeout(() => {
        flag = true;
      }, limit);
    }
  };
};

const throttledFetchData = throttle(fetchData, 500);

throttledFetchData();
throttledFetchData();
throttledFetchData();
```

## 簡表對比

| 特性         | Debounce                      | Throttle               |
| ------------ | ----------------------------- | ---------------------- |
| 執行時機     | 最後一次操作結束 N 毫秒後觸發 | 每隔 N 毫秒觸發一次    |
| 常見用途     | 搜尋建議、自動儲存            | 滾動加載、拖拉、resize |
| 是否跳過事件 | 可能（只執行最後一次）        | 是（中間事件不處理）   |
