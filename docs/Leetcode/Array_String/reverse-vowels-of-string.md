---
sidebar_position: 5
---

# 345. Reverse Vowels of a String

[Leetcode Link](https://leetcode.com/problems/reverse-vowels-of-a-string/)

Question:
Given a string s, reverse only all the vowels in the string and return it.

The vowels are 'a', 'e', 'i', 'o', and 'u', and they can appear in both lower and upper cases, more than once.

Example:

```
Input: s = "IceCreAm"

Output: "AceCreIm"

Explanation:

The vowels in s are ['I', 'e', 'e', 'A']. On reversing the vowels, s becomes "AceCreIm".
```

```jsx title="reverse-vowels-of-string"
var reverseVowels = function (s) {
  let vowels = ["a", "e", "i", "o", "u", "A", "E", "I", "O", "U"];

  let list = Array.from(s);
  let indices = [];
  let chars = [];

  for (let i = 0; i < list.length; i++) {
    if (vowels.includes(list[i])) {
      indices.push(i);
      chars.push(list[i]);
    }
  }

  chars.reverse();
  for (let i = 0; i < indices.length; i++) {
    list[indices[i]] = chars[i];
  }

  return list.join("");
};
```
