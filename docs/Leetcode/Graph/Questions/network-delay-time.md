---
sidebar_position: 1
---

# Network Delay Time

[Leetcode Link](https://leetcode.com/problems/network-delay-time/)

### 題目中文意思（白話版）

有一個網路，裡面有 **n 個節點**（編號從 1 到 n）。  
給你一個列表 `times`，裡面每一項 `[ui, vi, wi]` 表示：  
從節點 **ui** 發送信號到節點 **vi** 需要花 **wi** 時間（這是有向邊，只能從 ui → vi，不能反過來）。

現在，你從節點 **k** 開始發送信號。  
請問：**最少需要多少時間**，才能讓 **所有 n 個節點** 都收到這個信號？  
如果有的節點永遠收不到，就返回 **-1**。

**關鍵理解**：

- 信號會沿著邊傳播，每條邊都有傳遞時間。
- 每個節點收到信號的時間 = 從 k 到該節點的 **最短路徑時間**。
- 我們要找的是「最後一個收到信號的節點」所花的時間（也就是所有最短時間中的 **最大值**）。
- 如果有任何一個節點從 k 走不到，就不可能所有節點都收到 → 返回 -1。

### 舉例說明（Example 1）

輸入：  
`times = [[2,1,1], [2,3,1], [3,4,1]]`， `n = 4`， `k = 2`

圖形如下：

- 2 → 1 需要 1 時間
- 2 → 3 需要 1 時間
- 3 → 4 需要 1 時間

從節點 2 開始發信號：

- 節點 2：時間 0（自己立刻就有）
- 節點 1：從 2 直接過來，時間 1
- 節點 3：從 2 直接過來，時間 1
- 節點 4：從 2 → 3 → 4，時間 1 + 1 = 2

所以最後一個節點（4）在時間 **2** 收到信號 → 答案是 **2**

### 為什麼要用最短路徑？

因為信號可以走很多條路，我們永遠想走「最快抵達」的那條路。  
這就是單源最短路徑問題（起點固定為 k）。

推薦算法：**Dijkstra 算法**（因為所有 wi ≥ 0，正權重）

### 解題思路（簡單版）

1. 把 `times` 轉成圖：從每個節點 u 可以走到哪些 v，以及需要多少時間。
2. 用 Dijkstra 從 k 開始，計算 **k 到每個節點的最短時間**。
3. 看所有節點的最短時間，找出其中 **最大的那一個**（就是信號傳完整個網路的時間）。
4. 如果有節點永遠到不了（距離仍是無窮大），返回 -1。

### Python 程式碼（我之前給你的版本，附中文註解）

```python
import heapq
from collections import defaultdict

class Solution:
    def networkDelayTime(self, times, n: int, k: int) -> int:
        # 1. 建圖：graph[u] = [(v, 時間), ...]
        graph = defaultdict(list)
        for u, v, w in times:
            graph[u].append((v, w))

        # 2. dist[i] 表示從 k 到節點 i 的最短時間
        dist = [float('inf')] * (n + 1)
        dist[k] = 0

        # 3. 優先隊列（小頂堆）：存放 (目前時間, 節點)
        pq = [(0, k)]   # (time, node)

        while pq:
            time, node = heapq.heappop(pq)

            # 如果這個時間已經比之前找到的還差，就不用繼續了
            if time > dist[node]:
                continue

            # 放鬆所有鄰居
            for nei, w in graph[node]:
                new_time = time + w
                if new_time < dist[nei]:
                    dist[nei] = new_time
                    heapq.heappush(pq, (new_time, nei))

        # 4. 找出所有節點中最長的時間
        max_time = max(dist[1:])   # dist[0] 不用看
        return max_time if max_time != float('inf') else -1
```

### 常見困惑點解答

- **為什麼要取最大值，而不是總和？**  
  因為信號是同時向所有方向傳播的，最後收到信號的時間取決於「最慢的那條路徑」。

- **如果圖不連通怎麼辦？**  
  有些節點的 `dist[i]` 還是 `inf`，就返回 -1。

- **為什麼用優先隊列（heap）？**  
  每次都優先處理「目前已知最快抵達」的節點，這樣保證找到的是最短時間。

- **n ≤ 100，邊很多也沒關係**，這個解法效率很好。
