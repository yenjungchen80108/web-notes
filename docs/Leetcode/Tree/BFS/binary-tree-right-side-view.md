---
sidebar_position: 1
---

# 199. Binary Tree Right Side View

[Leetcode Link](https://leetcode.com/problems/binary-tree-right-side-view/)

Question:
Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

```
Input: root = [1,2,3,null,5,null,4]

Output: [1,3,4]
```

<figure>
    <img src="/img/leet/199-1.png" alt="Binary Tree Right Side View" width="400" />
</figure>

Example 2:

```
Input: root = [1,2,3,4,null,null,null,5]

Output: [1,3,4,5]
```

<figure>
    <img src="/img/leet/199-2.png" alt="Binary Tree Right Side View" width="400" />
</figure>

Example 3:

```
Input: root = [1,null,3]

Output: [1,3]
```

Example 4:

```
Input: root = []

Output: []
```

```js
var rightSideView = function (root) {
  // bfs
  // if (!root) return [];
  // const queue = [root]
  // const res = []

  // while (queue.length > 0) {
  //     let len = queue.length;

  //     for (let i = 0; i < len; i++) {
  //         const node = queue.shift()

  //         if (i === len - 1) res.push(node.val)

  //         if (node.left) queue.push(node.left)

  //         if (node.right) queue.push(node.right)
  //     }
  // }
  // return res

  // dfs
  const res = [];

  const dfs = (node, depth) => {
    if (!node) return [];

    if (res.length === depth) {
      res.push(node.val);
    }

    dfs(node.right, depth + 1);
    dfs(node.left, depth + 1);
  };

  dfs(root, 0);

  return res;
};
```
