---
sidebar_position: 9
---

# Heap Sort

用「Max Heap」來把陣列從小到大排序（因為每次把最大值丟到最後）。

## 思路整理：HeapSort with Max Heap

1. 先建立 Max Heap（heapify 所有父節點）
2. 然後從後往前：
   - 將堆頂（最大值）與末尾元素交換
   - 把 heap size 減一（末尾視為已排序區）
   - 對堆頂再執行 heapifyDown

## JavaScript 程式碼：原地 HeapSort

```js
function heapSort(arr) {
  const n = arr.length;

  // 建立 Max Heap（從最後一個父節點開始 heapify）
  for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {
    heapify(arr, n, i);
  }

  // 排序：每次將最大值（arr[0]）丟到最後面
  for (let end = n - 1; end > 0; end--) {
    // 交換最大值到最後
    [arr[0], arr[end]] = [arr[end], arr[0]];
    // 對縮小後的 heap 做 heapify
    heapify(arr, end, 0);
  }

  return arr;
}

// heapify(): 從 index 開始調整成合法 Max Heap
function heapify(arr, heapSize, index) {
  let largest = index;
  const left = 2 * index + 1;
  const right = 2 * index + 2;

  // 比較左子節點
  if (left < heapSize && arr[left] > arr[largest]) {
    largest = left;
  }

  // 比較右子節點
  if (right < heapSize && arr[right] > arr[largest]) {
    largest = right;
  }

  // 如果最大值不是自己 → 交換 & 遞迴下去
  if (largest !== index) {
    [arr[index], arr[largest]] = [arr[largest], arr[index]];
    heapify(arr, heapSize, largest);
  }
}
```

📌 範例測試：

```js
console.log(heapSort([4, 1, 3, 2])); // ➜ [1, 2, 3, 4]
console.log(heapSort([10, 5, 2, 7, 1])); // ➜ [1, 2, 5, 7, 10]
```
