---
sidebar_position: 1
---

# 374. Guess Number Higher or Lower

We are playing the Guess Game. The game is as follows:

I pick a number between 1 and n. You have to guess which number I picked.

Every time you guess wrong, I will tell you whether the number I picked is higher or lower than your guess.

```js
/**
 * Forward declaration of guess API.
 * @param {number} num   your guess
 * @return 	     -1 if num is higher than the picked number
 *			      1 if num is lower than the picked number
 *               otherwise return 0
 * var guess = function(num) {}
 */

/**
 * @param {number} n
 * @return {number}
 */
var guessNumber = function (n) {
  let left = 1,
    right = n;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    const res = guess(mid);

    if (res === 0) {
      return mid;
    } else if (res < 0) {
      right = mid - 1;
    } else {
      left = mid + 1;
    }
  }
};
```
