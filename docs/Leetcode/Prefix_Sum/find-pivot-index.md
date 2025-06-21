---
sidebar_position: 2
---

# 724. Find Pivot Index

[Leetcode Link](https://leetcode.com/problems/find-pivot-index)

```
Question:
回傳在 nums 中，pivot index 的 index。

Given an array of integers nums, calculate the pivot index of this array.

The pivot index is the index where the sum of all the numbers strictly to the left of the index is equal to the sum of all the numbers strictly to the index's right.

If the index is on the left edge of the array, then the left sum is 0 because there are no elements to the left. This also applies to the right edge of the array.

Return the leftmost pivot index. If no such index exists, return -1.

Example:

Input: nums = [1,7,3,6,5,6]
Output: 3
Explanation:
The pivot index is 3.
Left sum = nums[0] + nums[1] + nums[2] = 1 + 7 + 3 = 11
Right sum = nums[4] + nums[5] = 5 + 6 = 11
```

```jsx title="find-pivot-index"
var pivotIndex = function (nums) {
  let sum = nums.reduce((acc, curr) => acc + curr, 0);
  let leftSum = 0;

  for (let i = 0; i < nums.length; i++) {
    // 注意！要比較的時候，還沒加上 nums[i]
    if (leftSum === sum - leftSum - nums[i]) {
      return i;
    }
    leftSum += nums[i]; // 把 nums[i] 加進 leftSum
  }
  // total 是整個陣列的總和。
  // leftSum 是從頭到 i-1 的總和。
  // 當你走到 i：
  // 如果左邊總和 leftSum
  // 等於「剩下的右邊總和」= total - leftSum - nums[i]
  // 那麼 i 就是 Pivot！
  return -1;
};
```
