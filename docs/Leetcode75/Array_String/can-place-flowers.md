---
sidebar_position: 4
---

# 605. Can Place Flowers

[Leetcode Link](https://leetcode.com/problems/merge-strings-alternately/)

Question:
You are given two strings word1 and word2. Merge the strings by adding letters in alternating order, starting with word1. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return the merged string.

Example:

```
Input: word1 = "abc", word2 = "pqr"
Output: "apbqcr"
```

```jsx title="can-place-flowers"
var canPlaceFlowers = function (flowerbed, n) {
  let i = 0,
    count = 0;
  const len = flowerbed.length;

  while (i < len) {
    if (
      flowerbed[i] === 0 &&
      (i === 0 || flowerbed[i - 1] === 0) &&
      (i === len - 1 || flowerbed[i + 1] === 0)
    ) {
      flowerbed[i] = 1;
      count++;
      if (count >= n) return true;
      i += 2;
    } else {
      i += flowerbed[i] === 1 ? 2 : 1;
    }
  }

  return count >= n;
};
```
