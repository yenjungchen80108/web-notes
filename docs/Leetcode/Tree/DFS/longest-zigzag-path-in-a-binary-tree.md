---
sidebar_position: 5
---

# 1372. Longest ZigZag Path in a Binary Tree

[Leetcode Link](https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/)

Question:
You are given the root of a binary tree.

A ZigZag path for a binary tree is defined as follow:

Choose any node in the binary tree and a direction (right or left).
If the current direction is right, move to the right child of the current node; otherwise, move to the left child.
Change the direction from right to left or from left to right.
Repeat the second and third steps until you can't move in the tree.
Zigzag length is defined as the number of nodes visited - 1. (A single node has a length of 0).

Return the longest ZigZag path contained in that tree.

<figure>
    <img src="/img/leet/1372.png" alt="ZigZag Path" />
</figure>

```
Input: root = [1,null,1,1,1,null,null,1,1,null,1,null,null,null,1]
Output: 3
Explanation: Longest ZigZag path in blue nodes (right -> left -> right).
```

```js
var longestZigZag = function (root) {
  let maxLen = 0;

  var dfs = function (node, isLeft, length) {
    if (!node) return 0;

    maxLen = Math.max(maxLen, length);

    if (isLeft) {
      dfs(node.right, false, length + 1);
      // 從同一節點起一條新的path, 換方向從頭算, 避免漏掉別的錢再長path
      dfs(node.left, true, 1);
    } else {
      dfs(node.left, true, length + 1);
      dfs(node.right, false, 1);
    }
  };

  dfs(root.left, true, 1);
  dfs(root.right, false, 1);

  return maxLen;
};
```
