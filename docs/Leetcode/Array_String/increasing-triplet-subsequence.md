---
sidebar_position: 8
---

# 334. Increasing Triplet Subsequence

[Leetcode Link](https://leetcode.com/problems/increasing-triplet-subsequence/)

Question:
找出三個數字，使得第一個數字小於第二個數字，第二個數字小於第三個數字。如果有，回傳 true，否則回傳 false。

Given an integer array nums, return true if there exists a triple of indices (i, j, k) such that i < j < k and nums[i] < nums[j] < nums[k]. If no such indices exists, return false.

Example:

```
Input: nums = [1,2,3,4,5]
Output: true
Explanation: Any triplet where i < j < k is valid.
```

```
Input: nums = [2,1,5,0,4,6]
Output: true
Explanation: The triplet (3, 4, 5) is valid because nums[3] == 0 < nums[4] == 4 < nums[5] == 6.
```

```jsx title="increasing-triplet-subsequence"
function increasingKSubsequence(nums, k) {
  // k increase num arr length
  let incList = []; // incList[i] 儲存「長度為 i+1 的遞增子序列中，目前可達的最小結尾值」

  for (let n of nums) {
    let i = 0;
    // i 代表「目前 n 可以放入哪個長度的遞增子序列中」
    while (i < incList.length && incList[i] < n) i++;

    // 這段是在 incList 中找第一個「大於等於 n 的位置」
    if (i < incList.length) {
      // 如果找到一個位置 i，n 可以覆蓋 incList[i]
      incList[i] = n;
    } else {
      // 如果找不到位置（表示 n 比陣列裡所有值都大），那就可以讓 n 延長一段新的長度
      incList.push(n);
    }

    // 一旦 incList 的長度大於等於 k，代表我們找到了長度為 k 的遞增子序列
    if (incList.length >= k) return true;
  }

  return false;
}

var increasingTriplet = function (nums) {
  return increasingKSubsequence(nums, 3);
};
```
