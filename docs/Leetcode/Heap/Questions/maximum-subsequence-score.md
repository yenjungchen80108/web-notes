---
sidebar_position: 3
---

# 2542.Maximum Subsequence Score

[Leetcode Link](https://leetcode.com/problems/maximum-subsequence-score/)

```
Question:

You are given two 0-indexed integer arrays nums1 and nums2 of equal length n and a positive integer k. You must choose a subsequence of indices from nums1 of length k.

For chosen indices i0, i1, ..., ik - 1, your score is defined as:

The sum of the selected elements from nums1 multiplied with the minimum of the selected elements from nums2.
It can defined simply as: (nums1[i0] + nums1[i1] +...+ nums1[ik - 1]) \* min(nums2[i0] , nums2[i1], ... ,nums2[ik - 1]).
Return the maximum possible score.

A subsequence of indices of an array is a set that can be derived from the set {0, 1, ..., n-1} by deleting some or no elements.
```

```
Example 1:

Input: nums1 = [1,3,3,2], nums2 = [2,1,3,4], k = 3
Output: 12
Explanation:
The four possible subsequence scores are:

- We choose the indices 0, 1, and 2 with score = (1+3+3) \* min(2,1,3) = 7.
- We choose the indices 0, 1, and 3 with score = (1+3+2) \* min(2,1,4) = 6.
- We choose the indices 0, 2, and 3 with score = (1+3+2) \* min(2,3,4) = 12.
- We choose the indices 1, 2, and 3 with score = (3+3+2) \* min(1,3,4) = 8.
  Therefore, we return the max score, which is 12.
  Example 2:

Input: nums1 = [4,2,3,1,1], nums2 = [7,5,10,9,6], k = 1
Output: 30
Explanation:
Choosing index 2 is optimal: nums1[2] _ nums2[2] = 3 _ 10 = 30 is the maximum possible score.
```

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @param {number} k
 * @return {number}
 */
class MinHeap {
  constructor() {
    this.heap = [];
  }

  push(val) {
    this.heap.push(val);
    this.heapifyUp(this.heap.length - 1);
  }

  pop() {
    if (this.heap.length === 0) return null;
    if (this.heap.length === 1) return this.heap.pop();

    const min = this.heap[0]; // 把堆頂元素（最小值）先記下來
    this.heap[0] = this.heap.pop(); // 把最後一個元素拿來放到堆頂
    this.heapifyDown(0); // 從堆頂開始往下調整，使堆重新成為合法 min heap
    return min; // 回傳剛剛記下來的最小值
  }

  size() {
    return this.heap.length;
  }

  heapifyUp(index) {
    while (index > 0) {
      const parentIndex = Math.floor((index - 1) / 2);
      if (this.heap[parentIndex] <= this.heap[index]) break;

      [this.heap[parentIndex], this.heap[index]] = [
        this.heap[index],
        this.heap[parentIndex],
      ];
      index = parentIndex;
    }
  }

  heapifyDown(index) {
    while (true) {
      const length = this.heap.length;
      let largest = index;

      const left = 2 * index + 1;
      const right = 2 * index + 2;

      if (left < length && this.heap[left] < this.heap[largest]) {
        largest = left;
      }

      if (right < length && this.heap[right] < this.heap[largest]) {
        largest = right;
      }

      if (largest === index) break;

      [this.heap[index], this.heap[largest]] = [
        this.heap[largest],
        this.heap[index],
      ];
      index = largest;
    }
  }
}

var maxScore = function (nums1, nums2, k) {
  const n = nums1.length;
  const pairs = [];

  for (let i = 0; i < n; i++) {
    pairs.push([nums1[i], nums2[i]]);
  }

  pairs.sort((a, b) => b[1] - a[1]);

  const minHeap = new MinHeap();
  let heapSum = 0;
  let result = 0;

  for (let [num1, num2] of pairs) {
    minHeap.push(num1);
    heapSum += num1;

    if (minHeap.size() > k) {
      heapSum -= minHeap.pop();
    }

    if (minHeap.size() === k) {
      result = Math.max(result, heapSum * num2);
    }
  }

  return result;
};
```
