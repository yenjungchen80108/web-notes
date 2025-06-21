---
sidebar_position: 1
---

# 1768. Merge Strings Alternately

[Leetcode Link](https://leetcode.com/problems/merge-strings-alternately/)

Question:
You are given two strings word1 and word2. Merge the strings by adding letters in alternating order, starting with word1. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return the merged string.

Example:

```
Input: word1 = "abc", word2 = "pqr"
Output: "apbqcr"
```

```jsx title="merge-string"
var mergeAlternately = function (word1, word2) {
  let m = word1.length;
  let n = word2.length;

  let i = 0;
  let j = 0;
  let result = [];
  while (i < m || j < n) {
    if (i < m) {
      result.push(word1[i]);
      i += 1;
    }

    if (j < n) {
      result.push(word2[j]);
      j += 1;
    }
  }

  return result.join("");
};
```
