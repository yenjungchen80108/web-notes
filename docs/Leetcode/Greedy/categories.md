---
sidebar_position: 1
---

下面列出一些常見的貪心演算法範例，按照不同應用場景做分類簡介：

一、選擇／分配類

1. **活動選擇（Activity Selection）**

- 問題：給定多組活動，每個活動有開始與結束時間，選出相容（互不重疊）的最大數量。
- 貪心策略：每次選擇最早結束的活動，然後排除與之衝突的活動。

2. **區間調度最大化（Interval Scheduling Maximization）**

- 與活動選擇相同，只是名稱不同；也是依「結束時間最小」原則。

3. **作業排程最小延遲（Minimizing Maximum Lateness）**

- 問題：每個作業有處理時間與截止時間，安排順序使得最大延遲最小。
- 貪心策略：依照截止時間（deadline）由小到大排序執行。

二、背包與分配類

1. **分數背包（Fractional Knapsack）**

- 問題：每個物品有重量與價值，背包有容量限制，可拆分物品，要求價值最大。
- 貪心策略：先取「價值／重量」比最高的物品直到背包裝滿。

2. **工作報酬排程（Job Sequencing with Deadlines）**

- 問題：每個工作有截止時間與報酬，最多做一件，想最大化總報酬。
- 貪心策略：按報酬由高到低嘗試排入最晚可行時段（用並查集或優先佇列實作）。

三、圖論／最短路徑／最小生成樹

1. **Dijkstra 最短路徑**

- 每次選擇當前最短的那個節點「放鬆」其鄰邊，適用於非負邊權。

2. **Prim 最小生成樹（MST）**

- 問題：在加權無向圖中找到連通且權重總和最小的生成樹。
- 貪心策略：從一個起點開始，重複加入與當前樹相連且邊權最小的邊。

3. **Kruskal 最小生成樹（MST）**

- 貪心策略：將所有邊按權重由小到大排序，依序加入且避免形成環（用並查集檢查）。

四、編碼與壓縮

1. **Huffman 編碼**

- 問題：給定一組符號及其出現頻率，構建前綴碼使加權平均碼長最短。
- 貪心策略：反覆取出頻率最小的兩棵樹合併，構造二元樹。

五、幣值找零

1. **硬幣找零（Canonical Coin Systems）**

- 問題：給定幣值集合（如台幣、美元）與目標金額，找最少枚數硬幣。
- 貪心策略：每次用面額最大的幣，前提是該幣值系統滿足「貪心最優性」。

六、其他近似／啟發式

1. **集合覆蓋近似（Set Cover Approximation）**

- 問題：要覆蓋全部元素，挑最少子集；NP 難，貪心能達 O(\ln n) 近似。
- 策略：每步選擇能覆蓋最多「尚未覆蓋」元素的子集。

2. **任務分派近似（Load Balancing）**

- 問題：將任務分給多台機器，降低最大負載；貪心可取任務到當前負載最小的機器。

3. **最接近點對（Closest Pair）基於排序的簡易啟發式**

- 雖非最優（需 O(n\log n) 分治），但可用排序再線性掃描作近似。

這些演算法都遵循「在當前狀態下，做出當前看來最好的選擇」的貪心原則。不過使用前要先驗證問題是否滿足貪心選擇與無後顧性，以免導致非最優解。

⸻

一、區間調度／活動選擇（Activity Selection）

| 主題       | 題號 | 題名（中／英）                                                                                                                        | 要點                           |
| ---------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| 無重疊區間 | 435  | [無重疊區間 / Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/description/)                        | 按結束時間排序、貪心取最早結束 |
| 射爆氣球   | 452  | [用最少箭射爆氣球 / Minimum Number of Arrows…](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/) | 同上， interval scheduling     |
| 合併區間   | 56   | [合併區間 / Merge Intervals](https://leetcode.com/problems/merge-intervals/description/)                                              | 排序 + 線性掃描                |

二、具有截止日期的任務排程（Deadlines Scheduling）

| 主題                       | 題號 | 題名（中／英）                                                                                     | 要點                                              |
| -------------------------- | ---- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| 課程排程（最長可修課程數） | 630  | [課程表 III / Course Schedule III](https://leetcode.com/problems/course-schedule-iii/description/) | 按截止時間排序，超過總時長刪去最長任務 (max-heap) |

三、背包問題（Fractional / 0-1 Knapsack）

LeetCode 上 並無 Fractional Knapsack 的直接實作，可參考競賽平台或 HackerRank、AtCoder 題庫。不過以下 DP 題目也可熟悉背包思維：

416. 分割等和子集 / Partition Equal Subset Sum
417. 目標和 / Target Sum

四、單源最短路徑（Dijkstra）

| 主題         | 題號 | 題名（中／英）                                                                                                             | 要點                |
| ------------ | ---- | -------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| 網路延遲     | 743  | [網路延遲時間 / Network Delay Time](https://leetcode.com/problems/network-delay-time/description/)                         | Dijkstra 模板       |
| 限制中轉航班 | 787  | [限制中轉次數內最便宜航班 / Cheapest Flights…](https://leetcode.com/problems/cheapest-flights-within-k-stops/description/) | K-stop 最短路徑變形 |
| 最小努力值   | 1631 | [最小努力值路徑 / Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/description/)           | Dijkstra on grid    |

五、最小生成樹（MST: Prim/Kruskal）

| 主題               | 題號 | 題名（中／英）                                                                                                                     | 要點                |
| ------------------ | ---- | ---------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| 連接所有點最小成本 | 1584 | [連接所有點的最小費用 / Min Cost to Connect…](https://leetcode.com/problems/min-cost-to-connect-all-points/description/)           | Prim / Kruskal 均可 |
| 智慧供水           | 1168 | [優化村莊供水 / Optimize Water Distribution…](https://leetcode.com/problems/optimize-water-distribution-in-a-village/description/) | MST + 附加虛擬節點  |
| 連接城市成本       | 1135 | [連接城市的最小成本 / Connecting Cities…](https://leetcode.com/problems/connecting-cities-with-minimum-cost/description/)          | Kruskal + 並查集    |

六、Huffman 樣式合併（Greedy on Trees）

| 主題         | 題號 | 題名（中／英）                                                                                                                   | 要點                            |
| ------------ | ---- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| 最小合併成本 | 1167 | [連接棍子的最小成本 / Minimum Cost to Connect Sticks](https://leetcode.com/problems/minimum-cost-to-connect-sticks/description/) | 每次合併當前最短兩段（Huffman） |

七、找零問題（Coin Change / Change-Making）

| 主題           | 題號 | 題名（中／英）                                                                   | 要點                              |
| -------------- | ---- | -------------------------------------------------------------------------------- | --------------------------------- |
| 零錢兌換（DP） | 322  | [零錢兌換 / Coin Change](https://leetcode.com/problems/coin-change/description/) | DP 解法；貪心僅對特定幣值系統最優 |

八、其他常見貪心題

| 主題                 | 題號 | 題名（中／英）                                                                                                            | 要點                           |
| -------------------- | ---- | ------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| 分發餅乾             | 455  | [分發餅乾 / Assign Cookies](https://leetcode.com/problems/assign-cookies/description/)                                    | 排序 + 雙指標貪心              |
| 單次股票交易最佳利潤 | 121  | [買賣股票的最佳時機 / Best Time to…](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)          | 貪心追蹤最低價 + 差值更新      |
| 多次股票交易         | 122  | [買賣股票的最佳時機 II / Best Time…](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)       | 對每次上漲段求和               |
| 完成所有工作最短時間 | 1723 | [完成所有工作的最短時間 / Find Minimum…](https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs/description/) | LPT（Longest Processing Time） |

⸻

以上題目基本涵蓋了常見的貪心套路，推薦你在刷題時刻意分類練習，並在解題後反思「為何當前選擇最優＝整體最優」的正當性。祝練習順利！
