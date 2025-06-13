---
sidebar_position: 2
---

# 283. Move Zeroes

[Leetcode Link](https://leetcode.com/problems/move-zeroes/)

Question:
Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Note that you must do this in-place without making a copy of the array.

Example:

```
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

```jsx title="move-zeros"
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function (nums) {
  let low = 0;
  let high = low + 1;

  while (high <= nums.length - 1) {
    if (nums[low] !== 0) {
      low++;
      high++;
    } else {
      if (nums[high] !== 0) {
        [nums[low], nums[high]] = [nums[high], nums[low]];
        low++;
      }
      high++;
    }
  }
};
```
