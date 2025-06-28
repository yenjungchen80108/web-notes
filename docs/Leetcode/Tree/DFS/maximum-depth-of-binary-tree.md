---
sidebar_position: 1
---

# 104. Maximum Depth of Binary Tree

[Leetcode Link](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

Question:
Given the root of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Example 1:

<figure>
    <img src="/img/leet/104.jpg" alt="Maximum Depth of Binary Tree" width="400" />
</figure>

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

Example 2:

```
Input: root = [1,null,2]
Output: 2
```

```js
var maxDepth = function (root) {
  if (root === null) return 0;

  const leftDepth = maxDepth(root.left);
  const rightDepth = maxDepth(root.right);

  return Math.max(leftDepth, rightDepth) + 1;
};
```
