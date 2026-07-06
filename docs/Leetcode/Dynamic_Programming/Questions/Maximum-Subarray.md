---
sidebar_position: 1
---

# Maximum Subarray

[Leetcode Link](https://leetcode.com/problems/maximum-subarray/)

```
Question:

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
```

### 1. 解決什麼問題？

**最大子陣列加總 (Maximum Subarray Sum)**：
在一個包含正數與負數的整數陣列中，找出一組「連續」的數字，讓它們的和最大。

- **範例陣列：** `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`
- **最大子陣列：** `[4, -1, 2, 1]`
- **總和：** **6**

### 2. Kadane 的核心邏輯 (The Logic)

Kadane 的天才之處在於：**我每走到一個位置，都只問一個問題。**

假設你走到了索引 $i$，你有兩個選擇：

1.  **加入前面的隊伍：** 把當前的數字 $A[i]$ 加到「以 $i-1$ 結尾的最大和」裡。
2.  **另起爐灶：** 如果前面的隊伍已經變成負數（累贅），那就從 $A[i]$ 自己重新開始。

**數學公式：**
$$local\_max[i] = \max(A[i], A[i] + local\_max[i-1])$$

最後，我們只要記錄過程出現過最大的那個 $local\_max$，就是我們要的答案。

### 3. 為什麼它很神？ (Complexity)

- **暴力解 (Brute Force):** 檢查所有組合，$O(n^2)$。
- **Kadane's:** 只需要掃描一遍陣列，$O(n)$。
- **空間複雜度:** $O(1)$，你只需要兩個變數（`current_max` 和 `global_max`）。

### 4. 偽代碼 (Python 範例)

這就是你在 KT 那本書或維基百科上會看到的簡潔邏輯：

```python
def max_subarray(nums):
    curr_max = global_max = nums[0]

    for i in range(1, len(nums)):
        # 決定要「另起爐灶」還是「跟著前輩混」
        curr_max = max(nums[i], curr_max + nums[i])

        # 更新目前看過的最高紀錄
        if curr_max > global_max:
            global_max = curr_max

    return global_max
```

**問題描述**：  
給定一個整數陣列 `nums`，找出 **連續子陣列**（subarray）中和最大的那一個，並返回其總和。

**範例**：

```python
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
# 最大子陣列為 [4, -1, 2, 1]，和 = 6
```

### 1. Kadane's Algorithm（最推薦，O(n) 時間、O(1) 空間）

這是經典解法，也是 Greedy + DP 的優化版本。

**核心思路**：

- 維護一個 `current_sum`：代表「以當前位置結尾」的最大子陣列和。
- 每一步決定：是繼續加上前面的子陣列，還是從當前元素重新開始。
- 同時記錄全局最大值 `max_sum`。

```python
def maxSubArray(nums):
    if not nums:
        return 0

    max_sum = current_sum = nums[0]   # 初始化為第一個元素

    for i in range(1, len(nums)):
        # 關鍵決策：要不要從 nums[i] 重新開始
        current_sum = max(nums[i], current_sum + nums[i])
        max_sum = max(max_sum, current_sum)

    return max_sum
```

**優點**：最簡潔、最高效  
**時間複雜度**：O(n)  
**空間複雜度**：O(1)

### 2. 動態規劃（Dynamic Programming） - 標準版

使用 `dp[i]` 表示「以 `i` 為結尾的最大子陣列和」。

```python
def maxSubArray_dp(nums):
    if not nums:
        return 0

    n = len(nums)
    dp = [0] * n
    dp[0] = nums[0]
    max_sum = nums[0]

    for i in range(1, n):
        dp[i] = max(nums[i], dp[i-1] + nums[i])
        max_sum = max(max_sum, dp[i])

    return max_sum
```

**空間優化版**（與 Kadane 本質相同）：
把 `dp` 陣列壓縮成一個變數，就是上面的 Kadane 寫法。

### 3. Greedy（貪婪）寫法

這是 Kadane 的另一種常見表達形式（遇到負數就重置）：

```python
def maxSubArray_greedy(nums):
    if not nums:
        return 0

    max_sum = float('-inf')
    current_sum = 0

    for num in nums:
        current_sum += num
        max_sum = max(max_sum, current_sum)

        # 如果目前累加變成負數，重新開始（貪婪選擇）
        if current_sum < 0:
            current_sum = 0

    return max_sum
```

**注意**：這個版本在**全為負數**的情況下會出錯（例如 `nums = [-1]` 應返回 -1，但此版本可能返回 0）。  
**正確做法**還是建議使用第一種 Kadane 寫法（`current_sum = max(nums[i], current_sum + nums[i])`）。

### 三種解法比較表

| 解法               | 時間複雜度 | 空間複雜度  | 是否處理全負數 | 可讀性 | 推薦程度 |
| ------------------ | ---------- | ----------- | -------------- | ------ | -------- |
| Kadane's Algorithm | O(n)       | O(1)        | 是             | 高     | ★★★★★    |
| 動態規劃 (DP)      | O(n)       | O(n) / O(1) | 是             | 中     | ★★★★☆    |
| Greedy (重置版)    | O(n)       | O(1)        | 否（需修改）   | 高     | ★★★☆☆    |

### 完整類別寫法（LeetCode 提交格式）

```python
from typing import List

class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        max_sum = current_sum = nums[0]

        for i in range(1, len(nums)):
            current_sum = max(nums[i], current_sum + nums[i])
            max_sum = max(max_sum, current_sum)

        return max_sum
```

**小提醒**：

- Kadane's Algorithm 本質上是 **優化後的動態規劃**，同時帶有 Greedy 的決策（在每個位置做局部最優選擇）。
- 面試時通常會先問暴力法（O(n²)），再引導到 Kadane。
