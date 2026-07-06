---
sidebar_position: 1
---

# Min Cost to Connect All Points

[Leetcode Link](https://leetcode.com/problems/min-cost-to-connect-all-points/)

### 1. 題目意思（白話版）

給你平面上 `n` 個點（座標 `points[i] = [xi, yi]`），  
你要把**全部的點連接起來**，形成一個**連通圖**，  
而且**不能有環**（也就是形成一棵「樹」）。

兩個點之間的連接成本是 **曼哈頓距離**：

```
cost(i,j) = |x_i - x_j| + |y_i - y_j|
```

問：**最小的總連接成本是多少？**

**本質**：這就是「**最小生成樹（Minimum Spanning Tree, MST）**」問題，只是邊的權重是曼哈頓距離。

---

### 2. 為什麼一定要用 MST？

- 如果有環 → 代表有「多餘」的邊，可以刪掉降低成本
- 我們只需要 `n-1` 條邊就能把 `n` 個點全部連起來
- 目標：選 `n-1` 條邊，讓總成本最小，且圖仍然連通

---

### 3. 兩種經典解法（推薦順序）

| 解法                 | 時間複雜度  | 適合 n=1000 | 推薦指數 |
| -------------------- | ----------- | ----------- | -------- |
| Kruskal + Union-Find | O(n² log n) | 非常適合    | ★★★★★    |
| Prim's + 優先佇列    | O(n²)       | 適合        | ★★★★☆    |

**強烈推薦 Kruskal**（因為程式碼最清晰、容易理解）

---

### 4. Kruskal 演算法一步一步拆解（最推薦）

https://xlinux.nist.gov/dads/HTML/kruskalsalgo.html

#### 步驟 1：把所有可能的邊都列出來

- 每兩個點之間都有一條邊
- 計算其曼哈頓距離
- 存成 `(距離, i, j)` 的形式

```python
edges = []
for i in range(n):
    for j in range(i+1, n):
        dist = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1])
        edges.append((dist, i, j))
```

#### 步驟 2：把邊按照距離由小到大排序

```python
edges.sort()        # 預設會先按第一個元素（距離）排序
```

#### 步驟 3：使用 Union-Find（並查集）避免形成環

- 一開始每個點都是獨立的集合
- 每次挑最短的邊，如果兩個點不在同一個集合，就把這條邊加入 MST，並把兩個集合合併
- 直到我們加了 `n-1` 條邊為止

#### 步驟 4：實作 Union-Find（路徑壓縮 + 按秩合併）

---

### 5. 完整 Python 程式碼（Kruskal 版）

```python
class Solution:
    def minCostConnectPoints(self, points):
        n = len(points)
        if n == 1:
            return 0

        # 步驟1：建立所有邊
        edges = []
        for i in range(n):
            for j in range(i + 1, n):
                dist = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1])
                edges.append((dist, i, j))

        # 步驟2：排序
        edges.sort()

        # Union-Find
        parent = list(range(n))
        rank = [0] * n

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])   # 路徑壓縮
            return parent[x]

        def union(x, y):
            px, py = find(x), find(y)
            if px == py:
                return False
            if rank[px] < rank[py]:
                parent[px] = py
            elif rank[px] > rank[py]:
                parent[py] = px
            else:
                parent[py] = px
                rank[px] += 1
            return True

        # 步驟3：Kruskal 主迴圈
        cost = 0
        edges_used = 0

        for dist, i, j in edges:
            if union(i, j):
                cost += dist
                edges_used += 1
                if edges_used == n - 1:
                    break

        return cost
```

---

### 6. Prim's 演算法簡單版（供參考）

如果你想練習 Prim's，也可以這樣寫（使用優先佇列）：

```python
import heapq

class Solution:
    def minCostConnectPoints(self, points):
        n = len(points)
        if n == 1: return 0

        visited = [False] * n
        min_heap = [(0, 0)]   # (距離, 點的索引)
        total_cost = 0
        edges_used = 0

        while edges_used < n:
            cost, u = heapq.heappop(min_heap)
            if visited[u]:
                continue
            visited[u] = True
            total_cost += cost
            edges_used += 1

            for v in range(n):
                if not visited[v]:
                    dist = abs(points[u][0] - points[v][0]) + abs(points[u][1] - points[v][1])
                    heapq.heappush(min_heap, (dist, v))

        return total_cost
```

---

### 7. 時間複雜度總結

- **Kruskal**：建立邊 O(n²)，排序 O(n² log n) → 整體可接受（n=1000 時約 0.5 秒）
- **Prim's**：O(n²) → 更穩定，但程式碼較長

---

### 8. 常見考點提醒

- 這題是 **MST 經典變形**
- 一定要會 **Kruskal + Union-Find**（面試最常被問）
- 曼哈頓距離是關鍵（不是歐幾里德距離）
- n ≤ 1000，所以 O(n²) 是允許的

---

**想看哪一個部分更詳細？**

1. Union-Find 原理圖解
2. 範例一步一步執行過程（Example 1）
3. Prim's 完整帶註解版
4. 優化技巧（例如只考慮 x、y 排序後的邊）

告訴我你想深入哪一部分，我立刻補充！
