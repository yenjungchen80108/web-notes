---
sidebar_position: 2
---

# 872. Leaf-Similar Trees

[Leetcode Link](https://leetcode.com/problems/leaf-similar-trees/)

Question:
Consider all the leaves of a binary tree, from left to right order, the values of those leaves form a leaf value sequence.

<figure>
    <img src="/img/leet/872.png" alt="Leaf Similar Trees" width="500" />
</figure>

For example, in the given tree above, the leaf value sequence is (6, 7, 4, 9, 8).

Two binary trees are considered leaf-similar if their leaf value sequence is the same.

Return true if and only if the two given trees with head nodes root1 and root2 are leaf-similar.v

Example 1:

<figure>
    <img src="/img/leet/872-1.jpg" alt="Leaf Similar Trees" width="500" />
</figure>

```
Input: root1 = [3,5,1,6,2,9,8,null,null,7,4], root2 = [3,5,1,6,7,4,2,null,null,null,null,null,null,9,8]
Output: true
```

Example 2:

<figure>
    <img src="/img/leet/872-2.jpg" alt="Leaf Similar Trees" width="500" />
</figure>
```
Input: root1 = [1,2,3], root2 = [1,3,2]
Output: false
```

```js
var getLeaves = function (node, arr) {
  if (!node) return;

  if (!node.left && !node.right) {
    arr.push(node.val);
  }

  getLeaves(node.left, arr);
  getLeaves(node.right, arr);
};

var arrayEqual = function (a, b) {
  if (a.length !== b.length) return false;

  return a.every((val, i) => val === b[i]);
};

var leafSimilar = function (root1, root2) {
  let arr1 = [];
  let arr2 = [];

  getLeaves(root1, arr1);
  getLeaves(root2, arr2);

  return arrayEqual(arr1, arr2);
};
```
