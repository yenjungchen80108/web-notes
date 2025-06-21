---
sidebar_position: 6
---

# 151. Reverse Words in a String

[Leetcode Link](https://leetcode.com/problems/reverse-words-in-a-string/)

Question:
Given an input string s, reverse the order of the words.

A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

Example:

```
Input: s = "the sky is blue"
Output: "blue is sky the"
```

```jsx title="merge-string"
var reverseWords = function (s) {
  let test = s.split(" ");
  let left = 0;
  let right = test.length - 1;

  while (left < right) {
    [test[left], test[right]] = [test[right], test[left]];
    left++;
    right--;
  }

  return test.filter(String).join(" ");
  // return s.trim().split(/\s+/).reverse().join(" ");
};
```
