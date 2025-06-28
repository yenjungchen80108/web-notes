---
sidebar_position: 1
---

你會用到 -Infinity（或 Infinity）的情境，基本上都與「找最大／最小值的初始化」有關，尤其在：
• 比較大小前還不知道初始值
• 所有候選值都可能為負數或極端數值
• 需要保證第一次比較必定成立

⸻

✅ -Infinity 典型應用場景整理

① 找最大值問題（Max Problem）

🔹 題型舉例：

- LeetCode 53 – Maximum Subarray（Kadane’s Algorithm）
- LeetCode 124 – Binary Tree Maximum Path Sum
- LeetCode 1161 – Maximum Level Sum of a Binary Tree
- DP 題中的「最大總和 / 路徑最大值」

🔸 為什麼用 -Infinity？

因為所有數值可能都是負數，你不能預設為 0。

```js
let max = -Infinity;
for (let val of nums) {
  max = Math.max(max, val); // 第一次就會進來
}
```

⸻

② Backtracking / DFS 類比大小時

例如走迷宮、樹狀遞迴找最大深度、最大路徑和、最遠距離等：

```js
function dfs(node) {
  if (!node) return 0;
  let left = dfs(node.left);
  let right = dfs(node.right);
  maxPath = Math.max(maxPath, left + right + node.val); // maxPath 初始為 -Infinity
  return Math.max(left, right) + node.val;
}
```

⸻

③ 動態規劃（DP）中最大值初始化

```js
// 最大獲利路徑，初始化 dp table
let dp = Array(n).fill(-Infinity);
dp[0] = 0; // 起點
```

這樣才能讓狀態轉移時用 dp[i] = Math.max(dp[i], dp[j] + val) 保證正確。

⸻

④ 找最大差距 / 最大區間問題

如 LeetCode 121 – Best Time to Buy and Sell Stock：

```js
let minPrice = Infinity;
let maxProfit = -Infinity;
```

你用 -Infinity 來初始化 maxProfit，以確保：

- 第一次比較能正確進入
- 有時所有價格都下跌，最大獲利會是負值，也不會被 0 誤導

```js
let minPrice = Infinity;
let maxProfit = -Infinity;
```

⸻

⑤ 用 Priority Queue / Heap 做比較

有時你需要初始化優先順序（尤其是自定義 comparator）：

```js
const maxHeap = new PriorityQueue((a, b) => b - a);
let best = -Infinity; // 紀錄 heap 裡最大可能值
```

⸻

✅ 加碼補充：Infinity 什麼時候用？

同理，當你在找 最小值 時，要初始化為 Infinity：

```js
let minVal = Infinity;
for (let val of nums) {
  minVal = Math.min(minVal, val);
}
```

⸻

🎯 記憶小技巧

問題類型 初始值常用
找最大值 let max = -Infinity
找最小值 let min = Infinity
判斷是否進入過 loop let best = null/undefined（需額外處理）
DP 無效狀態填充 -Infinity / Infinity（視需求而定）

⸻

✅ 結論

使用 -Infinity 不只是語法選項，而是一種思維習慣：

「當你還不知道最大值是什麼，但又不想讓某些錯誤值（如 0、null）干擾時，就先用 -Infinity 安全保底。」

這個技巧讓你在 LeetCode 上解題時可以寫出更穩、更正確的邏輯，也避免邊界值造成的隱性 bug。
你如果願意，我可以幫你整理成一張「常用初始化策略對照表」，放在你 DocuSource 筆記的初始化篇裡。需要嗎？📘
