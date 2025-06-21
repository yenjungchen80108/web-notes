---
sidebar_position: 7
---

# 238. Product of Array Except Self

[Leetcode Link](https://leetcode.com/problems/product-of-array-except-self/)

Question:
回傳一個陣列，其中 answer[i] 等於 nums 中除了 nums[i] 以外的所有元素的乘積。

Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].

The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operation.

Example:

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

```jsx title="product-of-array-except-self"
var productExceptSelf = function (nums) {
  const n = nums.length;
  const res = new Array(n).fill(1);

  let prefix = 1;
  for (let i = 0; i < nums.length; i++) {
    res[i] = prefix;
    prefix *= nums[i];
  }

  let suffix = 1;
  for (let i = n - 1; i >= 0; i--) {
    res[i] *= suffix;
    suffix *= nums[i];
  }

  return res;
};
```
