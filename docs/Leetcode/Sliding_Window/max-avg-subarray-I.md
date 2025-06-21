---
sidebar_position: 1
---

# 643. Maximum Average Subarray I

[Leetcode Link](https://leetcode.com/problems/maximum-average-subarray-i/)

Question:

You are given an integer array nums consisting of n elements, and an integer k.

Find a contiguous subarray whose length is equal to k that has the maximum average value and return this value. Any answer with a calculation error less than 10-5 will be accepted.

Example:

```
Input: nums = [1,12,-5,-6,50,3], k = 4
Output: 12.75000
```

```jsx title="max-avg-subarray-I"
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var findMaxAverage = function (nums, k) {
  // in the k window elements, find the max val
  let sum = 0;
  // the first 4 elements accumulation
  for (let i = 0; i < k; i++) {
    sum += nums[i];
  }

  let maxSum = sum;

  for (let i = k; i < nums.length; i++) {
    sum = sum - nums[i - k] + nums[i];
    maxSum = Math.max(maxSum, sum);
  }

  return maxSum / k;
};
```
