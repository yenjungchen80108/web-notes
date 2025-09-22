---
sidebar_position: 3
---

# Permutations

## Question

Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

```text
Example 1:

Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
Example 2:

Input: nums = [0,1]
Output: [[0,1],[1,0]]
```

```js
var permute = function (nums) {
  if (!nums) return [];
  let res = [];

  function dfs(curr, remain) {
    if (remain.length === 0) {
      res.push([...curr]);
      return;
    }

    for (let i = 0; i < remain.length; i++) {
      const chosen = remain[i];

      curr.push(chosen);

      const newRemain = remain.slice(0, i).concat(remain.slice(i + 1));
      dfs(curr, newRemain);
      curr.pop();
    }
  }

  dfs([], nums);
  return res;
};
```
