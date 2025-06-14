---
sidebar_position: 2
---

# 1456. Maximum Number of Vowels in a Substring of Given Length

[Leetcode Link](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)

Question:

Given a string s and an integer k, return the maximum number of vowel letters in any substring of s with length k.

Vowel letters in English are 'a', 'e', 'i', 'o', and 'u'.

Example:

```
Input: s = "abciiidef", k = 3
Output: 3
Explanation: The substring "iii" contains 3 vowel letters.
```

```jsx title="max-num-vowels-given-length"
var maxVowels = function (s, k) {
  let max = 0;
  let count = 0;
  let vowels = new Set(["a", "e", "i", "o", "u"]);

  for (let i = 0; i < k; i++) {
    if (vowels.has(s[i])) count++;
  }
  // the first k characters
  max = count;

  for (let i = k; i < s.length; i++) {
    // add the new character to the window
    if (vowels.has(s[i])) count++;
    // remove the first character of the window
    if (vowels.has(s[i - k])) count--;
    max = Math.max(max, count);
  }

  return max;
};
```
