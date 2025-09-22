---
sidebar_position: 3
---

# Subset

## Question

Given an integer array nums of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

```text
Example 1:

Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
Example 2:

Input: nums = [0]
Output: [[],[0]]
```

```js
var subsets = function (nums) {
  let res = [];

  const dfs = (curr, remain) => {
    res.push([...curr]);

    for (let i = 0; i < remain.length; i++) {
      let chosen = remain[i];

      curr.push(chosen);

      let newRemain = remain.slice(i + 1);

      dfs(curr, newRemain);
      curr.pop();
    }
  };

  dfs([], nums);
  return res;
};
```
