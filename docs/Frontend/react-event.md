---
sidebar_position: 12
---

# React Event

## 1. React 事件系統是什麼？

在 React 裡，你不會直接用原生 DOM API 去寫事件監聽器（例如 `button.addEventListener('click', ...)`）。
React 幫你建立了一層自己的 事件系統，所有事件都包裝成一個統一的格式，這就是 合成事件 (SyntheticEvent)。

這麼做的原因：

- 跨瀏覽器相容性：不同瀏覽器的事件物件屬性不完全一樣，React 幫你統一 API。
- 效能：React 用事件委派，避免在大量元素上各自掛 listener，減少記憶體消耗。
- 可控性：事件可以和 React 的批次更新 (batching) 機制一起工作。

## 2. 合成事件 (SyntheticEvent)

- 你在 React 裡拿到的 event，其實不是原生的 DOM Event，而是 React 包裝過的 合成事件物件。
- 例如：

```js
function App() {
  const handleClick = (e) => {
    console.log(e.type); // "click"
    console.log(e.nativeEvent); // 真正的原生事件
  };
  return <button onClick={handleClick}>Click me</button>;
}
```

- 特點：
  - 長得像 DOM Event，但有統一屬性。
  - 有 nativeEvent 屬性指向真實事件。
  - React 可能會重複使用事件物件（事件池機制，16+ 版本後逐步取消），所以常見的建議是 異步存取要先 e.persist() 或先拷貝屬性。

## 3. 事件委派 (Event Delegation)

- React 並不是在每個 DOM 節點上直接掛監聽器，而是把事件統一掛在 根容器（React 18 前是 document，React 18 改為 React root container 上）。
- 當某個子元素觸發事件，事件會 冒泡 到根容器，再由 React 分發到對應的 handler。
- 這就是 事件委派：一個上層負責管理所有子元素的事件。

比喻：

- 傳統方式：每個座位都安排一個服務生，等客人舉手 → 浪費人力。
- React 方式：整個餐廳只有一個服務生在門口看，誰舉手他就去查對應座位 → 高效。

## 4. React 事件流程 (以 click 為例)

    1.	使用者在 `<button>` 點擊。
    2.	事件傳到 DOM（nativeEvent）。
    3.	React 捕捉到冒泡到 root 的事件。
    4.	React 建立一個 SyntheticEvent。
    5.	React 執行對應的 handler（例如 onClick）。
    6.	事件結束，SyntheticEvent 可能被回收（事件池）。

⸻

## 5. 總結對照表

| 概念       | React 做法                                         |
| ---------- | -------------------------------------------------- |
| 事件物件   | 原生 Event → React 包裝成 SyntheticEvent，統一 API |
| 監聽器安裝 | React root 上集中安裝，再用事件委派分發            |
| 效能       | 減少記憶體佔用，提升效能                           |
| 相容性     | 不用管瀏覽器差異                                   |
| 特殊性     | 事件池（舊版），需要 e.persist() 才能保留事件物件  |
