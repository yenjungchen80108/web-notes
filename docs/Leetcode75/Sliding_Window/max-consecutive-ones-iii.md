---
sidebar_position: 3
---

# 1004. Max Consecutive Ones III

[Leetcode Link](https://leetcode.com/problems/max-consecutive-ones-iii/)

Question:
回傳在 nums 中，最多可以有 k 個 0 的連續 1 的長度。

Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.

Example:

```
Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6
Explanation: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

```jsx title="max-consecutive-ones-iii"
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var longestOnes = function (nums, k) {
  // 加一個 0，超過就縮；沒超過，就比長度。
  let zeroCount = 0;
  let l = 0;
  let maxLen = 0;

  for (let r = 0; r < nums.length; r++) {
    if (nums[r] === 0) zeroCount++;

    while (zeroCount > k) {
      // 當你「踢掉一個在視窗內的 0」，你也要從 zeroCount 中扣掉它，才不會多算
      // 當移除的是 0，就要把 zeroCount--，因為它不再屬於這個視窗了。
      if (nums[l] === 0) zeroCount--;
      l++;
    }

    maxLen = Math.max(maxLen, r - l + 1);
  }

  return maxLen;
};
```

longestOnes([1,1,1,0,0,0,1,1,1,1,0], 2) // 6
