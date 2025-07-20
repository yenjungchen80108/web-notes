---
sidebar_position: 1
---

# 215. Kth Largest Element in an Array

[Leetcode Link](https://leetcode.com/problems/kth-largest-element-in-an-array/)

Question:
Given an integer array nums and an integer k, return the kth largest element in the array.
Can you solve it without sorting?

為何不直接用 sort 來解？
因為 sort 的時間複雜度是 O(n log n)，而 heap 的時間複雜度是 O(k log n)，其中 k 是 heap 的大小。
所以如果 k 比較小，用 heap 來解會比較快。

Example:

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

## 解法一： 用 MaxHeap 來解

```jsx title="kth-largest-element-in-an-array"
class MaxHeap {
  constructor() {
    this.heap = []; // 9
  }

  _bubbleUp() {
    let index = this.heap.length - 1; // 最後index 8

    while (index > 0) {
      let parent = Math.floor((index - 1) / 2); // 中間index 4

      if (this.heap[parent] >= this.heap[index]) break;

      [this.heap[parent], this.heap[index]] = [
        this.heap[index],
        this.heap[parent],
      ];

      index = parent;
    }
  }

  _bubbleDown(index) {
    const length = this.heap.length;
    let largest = index;

    const left = 2 * index + 1;
    const right = 2 * index + 2;

    if (left < length && this.heap[left] > this.heap[largest]) {
      largest = left;
    }

    if (right < length && this.heap[right] > this.heap[largest]) {
      largest = right;
    }

    if (largest !== index) {
      [this.heap[largest], this.heap[index]] = [
        this.heap[index],
        this.heap[largest],
      ];
      this._bubbleDown(largest);
    }
  }

  insert(value) {
    this.heap.push(value);
    this._bubbleUp();
  }

  extractMax() {
    if (this.heap.length === 0) return null;
    if (this.heap.length === 1) return this.heap.pop();

    const max = this.heap[0];

    this.heap[0] = this.heap.pop();

    this._bubbleDown(0);

    return max;
  }
}

var findKthLargest = function (nums, k) {
  let hp = new MaxHeap();
  for (let i = 0; i < nums.length; i++) {
    hp.insert(nums[i]);
  }

  let result;
  for (let i = 0; i < k; i++) {
    result = hp.extractMax();
  }

  return result;
};
```

## 解法二： quick select

📌 原因一：平均時間複雜度比 Heap 快

- Heap 解法是：O(n log k)
- QuickSelect 平均為：O(n)（只處理一側 partition，而不是兩側）
- QuickSelect 是 QuickSort 的變體，不是為了排序整個陣列，而是「只找到第 k 大或第 k 小的元素」。

📌 原因二：考驗基本功（Partition 的思維）

- 快速排序中的 partition 是經典操作，快排能寫好代表你對資料結構與遞迴有一定理解。
- QuickSelect 加上一點小邏輯判斷就能解出 top-k 問題，很有效率。

⸻

🎯 QuickSelect 是什麼？一句話解釋：

就像 QuickSort，只是當我們知道答案一定在某一側的時候，我們就只遞迴那一側，因此比排序整個陣列還快。

✅ QuickSelect 步驟：

1. 隨機選一個 pivot（或固定選最後一個）
2. 將小於 pivot 的放左邊，大於等於 pivot 的放右邊
3. 看 pivot 在排序後是第幾大（例如 index = 3 → 表示是第 4 小）
4. 如果 pivotIndex == targetIndex，表示找到
5. 否則遞迴進到正確的半邊

```jsx title="kth-largest-element-in-an-array"
var findKthLargest = function (nums, k) {
  const target = nums.length - k; // 第 k 大 => 第 n - k 小

  const quickSelect = (left, right) => {
    const pivot = nums[right];
    let i = left;

    for (let j = left; j < right; j++) {
      if (nums[j] < pivot) {
        [nums[i], nums[j]] = [nums[j], nums[i]];
        i++;
      }
    }

    [nums[i], nums[right]] = [nums[right], nums[i]];
    // **就是把 pivot（右邊的值）放進排序好的中間點 i，讓 pivot 成為劃分左右陣列的「基準點」。

    if (i === target) return nums[i];
    else if (i < target) return quickSelect(i + 1, right);
    else return quickSelect(left, i - 1);
  };

  return quickSelect(0, nums.length - 1);
};
```
