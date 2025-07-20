---
sidebar_position: 1
---

# 700. Search in a Binary Search Tree

[Leetcode Link](https://leetcode.com/problems/search-in-a-binary-search-tree/)

Question:
You are given the root of a binary search tree (BST) and an integer val.

Find the node in the BST that the node's value equals val and return the subtree rooted with that node. If such a node does not exist, return null.

```
Input: root = [4,2,7,1,3], val = 2
```

```js
var searchBST = function (root, val) {
  if (!root) return null;

  if (root.val === val) {
    return root;
  } else if (val < root.val) {
    return searchBST(root.left, val);
  } else {
    return searchBST(root.right, val);
  }
};
```
