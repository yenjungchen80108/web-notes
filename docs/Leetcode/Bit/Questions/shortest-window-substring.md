---
sidebar_position: 1
---

# Shortest Balanced Substring

## Question

A string is considered "balanced" when every letter in the string appears both in uppercase and lowercase. For e.g., CATattac is balanced (a, c, t occur in both cases), while Madam is not (a, d only appear in lowercase). Write a function that, given a string, returns the shortest balanced substring of that string. For e.g.,:
“azABaabza” should return “ABaab”
“TacoCat” should return -1 (not balanced)
“AcZCbaBz” should returns the entire string

```js
function shortestBalancedSubstring(s) {
  const n = s.length;
  let best = null; // [start, endExclusive]

  // helper: map 'a'..'z' or 'A'..'Z' to 0..25, else -1
  function idx(ch) {
    const code = ch.charCodeAt(0);
    if (code >= 65 && code <= 90) return code - 65; // A-Z
    if (code >= 97 && code <= 122) return code - 97; // a-z
    return -1; // ignore non-letters (or treat as breaking chars if required)
  }

  for (let i = 0; i < n; i++) {
    let upper = 0,
      lower = 0;

    for (let j = i; j < n; j++) {
      const k = idx(s[j]);
      if (k !== -1) {
        const bit = 1 << k;
        if (s[j] >= "A" && s[j] <= "Z") upper |= bit;
        else lower |= bit;
      } else {
        // if non-letters should "break" balance, uncomment the next line:
        // upper = lower = 0;
      }

      // balanced iff every present letter appears in both cases
      if ((upper ^ lower) === 0) {
        if (best === null || j + 1 - i < best[1] - best[0]) {
          best = [i, j + 1];
          // early exit if length 2 (minimum for balance like "aA")
          if (best[1] - best[0] === 2) return s.slice(best[0], best[1]);
        }
      }
    }
  }
  return best ? s.slice(best[0], best[1]) : -1;
}

// Examples:
console.log(shortestBalancedSubstring("azABaabza")); // "ABaab"
console.log(shortestBalancedSubstring("TacoCat")); // -1
console.log(shortestBalancedSubstring("AcZCbaBz")); // "AcZCbaBz"
```

```python
def shortestBalancedSubstring(s: str) -> str:
    n = len(s)
    best = None

    for i in range(n):
        upper = 0
        lower = 0
        for j in range(i, n):
            ch = s[j]
            if ch.isalpha():
                k = ord(ch.lower()) - ord('a')
                if ch.isupper():
                    upper |= (1 << k)
                else:
                    lower |= (1 << k)

            if upper == lower and upper != 0:   # 重要：upper != 0 避免空字串
                if best is None or (j - i + 1) < (best[1] - best[0]):
                    best = (i, j + 1)
                    if (j - i + 1) == 2:         # 最小可能長度
                        return s[i:j+1]

    return s[best[0]:best[1]] if best else -1
```
