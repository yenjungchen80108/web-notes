---
sidebar_position: 2
---

# 1161. Maximum Level Sum of a Binary Tree

[Leetcode Link](https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree/)

Question:
Given the root of a binary tree, the level of its root is 1, the level of the its children is 2, and so on.
Return the smallest level x such that the sum of all the values of nodes at level x is maximal.

<figure>
    <img src="/img/leet/1161.jpeg" alt="Maximum Level Sum of a Binary Tree" width="400" />
</figure>

```
Input: root = [1,7,0,7,-8,null,null]
Output: 2
Explanation:
Level 1 sum = 1.
Level 2 sum = 7 + 0 = 7.
Level 3 sum = 7 + -8 = -1.
So we return the level with the maximum sum which is level 2.
```
