---
sidebar_position: 4
---

# Heap

## Heap 的定義

⸻

🔍 一、為什麼說 MinHeap / MaxHeap 在「查找最大或最小值」方面很方便？

✅ Heap 的特性：

- MinHeap：根節點永遠是整個樹的最小值
- MaxHeap：根節點永遠是整個樹的最大值

👉 因此：

- 查找最小值（MinHeap） → 直接回傳根節點 → O(1)
- 查找最大值（MaxHeap） → 直接回傳根節點 → O(1)
  Heap 不是 Binary Search Tree！它只保證「父子節點之間的大小關係」，但不保證整體有序。

```js
// MinHeap 查找最小值
const min = heap[0]; // O(1)
```

適用場景：

- Top K 問題
- 動態維護最大或最小值
- 優先佇列（Priority Queue）

⸻

🌳 二、Heap 與 Binary Search Tree 的主要差異？

| 比較點         | MinHeap / MaxHeap                              | Binary Search Tree (BST)                   |
| -------------- | ---------------------------------------------- | ------------------------------------------ |
| 結構規則       | 父節點與子節點有順序關係，但左右無特定大小順序 | 左 < 中 < 右，滿足中序遍歷為升序           |
| 最值查找       | O(1)，因為最值在根節點                         | O(log n) 平均、O(n) 最差，需走到最左或最右 |
| 插入/刪除      | O(log n)，維持堆積性質                         | O(log n) 平均、O(n) 最差（視平衡性）       |
| 可否搜尋任意值 | 不適合，因為左右子樹無序 → O(n)                | 適合搜尋任意值 → O(log n) 平均             |
| 平衡性         | 天生是平衡的完整二元樹                         | 可能不平衡（除非是 AVL / Red-Black Tree）  |

⸻

🧠 簡單總結：

✅ Heap 適合：

- 快速取出最大或最小值（O(1)）
- 不關心「中間某個值在哪裡」
- 資料不需要排序，只需要動態追蹤極值

✅ BST 適合：

- 搜尋任意值
- 資料需要有序（例如區間查詢、範圍篩選）
- 想用中序遍歷取得排序後的結果

⸻

✨ 圖像化理解（以 MaxHeap 與 BST 為例）

MaxHeap：

```js
     100
    /   \
  90     80
 / \     / \
70 60   50 40
```

- 查最大值：100（根）
- 查其他值：只能一層一層往下找 → O(n)

BST：

```js
      50
     /  \
   30    70
  / \    / \
20  40  60  80
```

- 查 80：從根一路往右走 → O(log n)
- 查最大值：右子節點走到底

⸻

🧪 結論：

- 你只想「快速知道誰是最小或最大」→ ✅ 用 Heap！
- 你想「找某個具體值、排序或做範圍查詢」→ ✅ 用 Binary Search Tree！

⸻

## Heap 是不是「只能」用來找極值？

✅ 基本上是的。

Heap（尤其是 Priority Queue）就是為了以下幾件事設計的：

- 快速找最大/最小值 → O(1)
- 插入 / 移除最大/最小值 → O(log n)
- 不需要關心順序，只在乎「誰最大/最小」

❌ 它 不是為了查找某個特定值設計，因為整個樹除了根之外是無序的。

⸻

🔧 那如果我既想找「最大/最小值」，又想「快速找特定值」怎麼辦？

是不是要結合 heap + binary search tree 的概念？

✅ 是的，有一些複雜資料結構就真的這麼做了！

⸻

🔁 常見的解法/延伸資料結構：

1️⃣ Treap（Tree + Heap 的 hybrid）

- 是一種同時滿足 BST & Heap 性質的資料結構。
- 維護 BST 性質：左小右大
- 維護 Heap 性質：每個節點有一個隨機優先值（維持平衡）
- 插入、刪除都可在平均 O(log n) 運作
- 👉 可以用來快速查值，同時保有平衡（比 AVL/Red-Black Tree 實作簡單）

⸻

2️⃣ HashMap + Heap 組合（實戰中常見）

- 在 LeetCode 或實際開發中，有時會這樣設計：
- HashMap: 讓你可以 O(1) 查找任意值是否存在
- Heap: 快速找出極值（最大或最小）

📦 範例場景：

- 記錄線上遊戲中所有玩家分數（找最高分又要找特定玩家）
- Top-K 頻率元素（你可能用 HashMap 統計 + Heap 排序）

⸻

🧠 小結對比

| 結構         | 查最大/最小 | 查任意值            | 插入        | 刪除最值        | 平衡性                    |
| ------------ | ----------- | ------------------- | ----------- | --------------- | ------------------------- |
| Heap         | ✅ O(1)     | ❌ O(n)             | ✅ O(log n) | ✅ O(log n)     | ✅ 完全二元樹             |
| BST          | ❌ O(log n) | ✅ O(log n)         | ✅ O(log n) | ❌ 需搜尋後刪除 | ❌（需 AVL or Red-Black） |
| Treap        | ✅ O(log n) | ✅ O(log n)         | ✅ O(log n) | ✅ O(log n)     | ✅ 自動平衡               |
| HashMap+Heap | ✅ O(1)     | ✅ O(1)（透過 Map） | 複合操作    | 複合操作        | -                         |

## Insert 操作（插入新節點）

⛓ 流程：

1. 把新值加到陣列的最後一個位置（維持完全二元樹）
2. 往上冒泡（bubble up / heapify up）：與父節點比較，若不符合 heap 性質就交換
3. 一直交換直到滿足 heap 條件

🧠 JavaScript MaxHeap 插入例子：

```js
class MaxHeap {
  constructor() {
    this.heap = [];
  }

  insert(value) {
    this.heap.push(value);
    this.bubbleUp();
  }

  bubbleUp() {
    let index = this.heap.length - 1;
    while (index > 0) {
      let parent = Math.floor((index - 1) / 2);
      if (this.heap[parent] >= this.heap[index]) break;
      [this.heap[parent], this.heap[index]] = [
        this.heap[index],
        this.heap[parent],
      ];
      index = parent;
    }
  }
}
```

## Delete 操作（刪除根節點）

⛓ 流程（以 MaxHeap 為例）：

1. 取出堆頂 this.heap[0]（最大值）
2. 將最後一個元素移到頂部
3. 往下沉（bubble down / heapify down）：與較大的子節點比較，若違反條件就交換
4. 直到恢復 heap 屬性

🧠 JavaScript MaxHeap 刪除頂部節點例子：

```js
removeMax() {
  if (this.heap.length === 0) return null;
  const max = this.heap[0];
  const end = this.heap.pop();
  if (this.heap.length > 0) {
    this.heap[0] = end;
    this.bubbleDown();
  }
  return max;
}

bubbleDown() {
  let index = 0;
  const length = this.heap.length;
  const element = this.heap[0];

  while (true) {
    let left = 2 * index + 1;
    let right = 2 * index + 2;
    let swap = null;

    if (left < length && this.heap[left] > element) {
      swap = left;
    }
    if (right < length &&
        this.heap[right] > (swap === null ? element : this.heap[left])) {
      swap = right;
    }

    if (swap === null) break;

    [this.heap[index], this.heap[swap]] = [this.heap[swap], this.heap[index]];
    index = swap;
  }
}
```

## Heap Sort（堆排序）

⛓ 流程：

1. 把原始陣列建立成 MaxHeap（建堆）
2. 把堆頂元素（最大值）與陣列末端交換
3. 排除最後一位後對剩下的陣列執行 bubbleDown（重建 heap）
4. 重複直到排序完成

🧠 JavaScript Heap Sort：

```js
function heapSort(arr) {
  const n = arr.length;

  // Build max heap
  for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {
    heapify(arr, n, i);
  }

  // Extract max one by one
  for (let i = n - 1; i > 0; i--) {
    [arr[0], arr[i]] = [arr[i], arr[0]]; // swap
    heapify(arr, i, 0); // rebuild heap
  }

  return arr;
}

function heapify(arr, n, i) {
  let largest = i;
  let left = 2 * i + 1;
  let right = 2 * i + 2;

  if (left < n && arr[left] > arr[largest]) largest = left;
  if (right < n && arr[right] > arr[largest]) largest = right;

  if (largest !== i) {
    [arr[i], arr[largest]] = [arr[largest], arr[i]];
    heapify(arr, n, largest);
  }
}
```

| 操作      | 說明                          | 時間複雜度 |
| --------- | ----------------------------- | ---------- |
| insert    | 插入新值並往上調整            | O(log n)   |
| delete    | 移除最大或最小值並往下調整    | O(log n)   |
| heap sort | 一次排序整個陣列（建堆+重建） | O(n log n) |
