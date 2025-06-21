---
sidebar_position: 3
---

# 2215. Find the Difference of Two Arrays

[Leetcode Link](https://leetcode.com/problems/find-the-difference-of-two-arrays/)

```
Question:

Given two 0-indexed integer arrays nums1 and nums2, return a list answer of size 2 where:

answer[0] is a list of all distinct integers in nums1 which are not present in nums2.
answer[1] is a list of all distinct integers in nums2 which are not present in nums1.
Note that the integers in the lists may be returned in any order.

Example 1:

Input: nums1 = [1,2,3], nums2 = [2,4,6]
Output: [[1,3],[4,6]]
Explanation:
For nums1, nums1[1] = 2 is present at index 0 of nums2, whereas nums1[0] = 1 and nums1[2] = 3 are not present in nums2. Therefore, answer[0] = [1,3].
For nums2, nums2[0] = 2 is present at index 1 of nums1, whereas nums2[1] = 4 and nums2[2] = 6 are not present in nums1. Therefore, answer[1] = [4,6].
```

```jsx title="find-the-difference-of-two-arrays"
let arr1 = new Set(nums1);
let arr2 = new Set(nums2);

let comNum1 = Array.from(arr1.difference(arr2).values()).map((value) => value);
let comNum2 = Array.from(arr2.difference(arr1).values()).map((value) => value);

// const diff1 = [...set1].filter(x => !set2.has(x));
// const diff2 = [...set2].filter(x => !set1.has(x));

return [comNum1, comNum2];
```
