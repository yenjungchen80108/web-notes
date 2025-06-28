---
sidebar_position: 4
---

# 605. Can Place Flowers

[Leetcode Link](https://leetcode.com/problems/can-place-flowers/)

Question:
You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in adjacent plots.

Given an integer array flowerbed containing 0's and 1's, where 0 means empty and 1 means not empty, and an integer n, return true if n new flowers can be planted in the flowerbed without violating the no-adjacent-flowers rule and false otherwise.

Example 1:

```
Input: flowerbed = [1,0,0,0,1], n = 1
Output: true
```

Example 2:

```
Input: flowerbed = [1,0,0,0,1], n = 2
Output: false
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
