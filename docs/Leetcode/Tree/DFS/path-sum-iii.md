---
sidebar_position: 4
---

# 437. Path Sum III

[Leetcode Link](https://leetcode.com/problems/path-sum-iii/)

Question:
Given the root of a binary tree and an integer targetSum, return the number of paths where the sum of the values along the path equals targetSum.
The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

Example 1:

<figure>
    <img src="/img/leet/437.jpg" alt="Path Sum III" width="500"  />
</figure>

```
Input: root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
Output: 3
Explanation: The paths that sum to 8 are shown.
```

Example 2:

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: 3
```

```js
// 內層 DFS：從某個節點出發，往下加總，看有沒有符合 target
// 一路從 target 減掉沿路經過的值，看有沒有某個節點 val === 剩下的值
var countPath = function (node, target) {
  if (!node) return 0;

  let match = node.val === target ? 1 : 0;

  return (
    match +
    countPath(node.left, target - node.val) +
    countPath(node.right, target - node.val)
  );
};

// 外層 DFS：對每個節點都試一次當「起點」

var pathSum = function (root, targetSum) {
  if (!root) return 0;

  return (
    countPath(root, targetSum) +
    pathSum(root.left, targetSum) +
    pathSum(root.right, targetSum)
  );
};
```

```js
// 累加版
var pathSum = function (root, targetSum) {
  if (!root) return 0;

  // 以每個節點當起點去找合法路徑
  return (
    countPathsFrom(root, 0, targetSum) +
    pathSum(root.left, targetSum) +
    pathSum(root.right, targetSum)
  );
};

var countPathsFrom = function (node, sumSoFar, target) {
  if (!node) return 0;

  sumSoFar += node.val;

  let count = sumSoFar === target ? 1 : 0;

  count += countPathsFrom(node.left, sumSoFar, target);
  count += countPathsFrom(node.right, sumSoFar, target);

  return count;
};
```
