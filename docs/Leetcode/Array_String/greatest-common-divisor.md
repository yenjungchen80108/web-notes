---
sidebar_position: 2
---

# 1071. Greatest Common Divisor of Strings

[Leetcode Link](https://leetcode.com/problems/greatest-common-divisor-of-strings/)

Question:
For two strings s and t, we say "t divides s" if and only if s = t + t + t + ... + t + t (i.e., t is concatenated with itself one or more times).

Given two strings str1 and str2, return the largest string x such that x divides both str1 and str2.

Example:

```
Input: str1 = "ABCABC", str2 = "ABC"
Output: "ABC"
```

```jsx title="greatest-common-divisor"
var gcdOfStrings = function (str1, str2) {
  const gcd = (a, b) => (b === 0 ? a : gcd(b, a % b));

  if (str1 + str2 != str2 + str1) return "";

  const gcdLen = gcd(str1.length, str2.length);

  return str1.substring(0, gcdLen);
};
```
