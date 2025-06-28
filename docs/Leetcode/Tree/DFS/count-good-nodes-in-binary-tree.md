---
sidebar_position: 3
---

# 1448. Count Good Nodes in Binary Tree

[Leetcode Link](https://leetcode.com/problems/count-good-nodes-in-binary-tree/)

Question:
Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.
Return the number of good nodes in the binary tree.

<figure>
    <img src="/img/leet/1448-1.png" alt="Count Good Nodes in Binary Tree" />
</figure>

```
Input: root = [3,1,4,3,null,1,5]
Output: 4
Explanation: Nodes in blue are good.
Root Node (3) is always a good node.
Node 4 -> (3,4) is the maximum value in the path starting from the root.
Node 5 -> (3,4,5) is the maximum value in the path
Node 3 -> (3,1,3) is the maximum value in the path.
```

<figure>
    <img src="/img/leet/1448-2.png" alt="Count Good Nodes in Binary Tree" />
</figure>

```
Input: root = [3,3,null,4,2]
Output: 3
Explanation: Node 2 -> (3, 3, 2) is not good, because "3" is higher than it.
```

```js
var getNodes = function (node, maxSoFar) {
  // 如果 node 是 null 就 return
  if (!node) return 0;

  let count = 0;
  // 如果 node.val >= maxSoFar，代表是 good node，就 count++
  if (node.val >= maxSoFar) {
    count++;
  }

  let newMax = Math.max(maxSoFar, node.val);

  // 然後往左遞迴，傳入更新後的 maxSoFar
  count += getNodes(node.left, newMax);
  // 再往右遞迴
  count += getNodes(node.right, newMax);

  return count;
};

var goodNodes = function (root) {
  return getNodes(root, root.val);
};
```
