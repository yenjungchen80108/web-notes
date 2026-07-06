---
sidebar_position: 1
---

# Graph

## 圖論問題的核心分類（只有這幾種！）

```
圖論問題 = 選擇「遍歷方式」 + 選擇「策略」
```

### 1. 遍歷方式（只有2種）

- **DFS** (深度優先，用遞迴或stack)
- **BFS** (廣度優先，用queue)

### 2. 核心策略

| 策略                 | 解決的問題           | 你做的題目                 |
| -------------------- | -------------------- | -------------------------- |
| **拓撲排序**         | 依賴關係、順序       | Course Schedule            |
| **MST** (最小生成樹) | 連接所有點的最小成本 | Min Cost to Connect Points |
| **最短路徑**         | 兩點間的最短距離     | Network Delay Time         |
| **環檢測**           | 檢查是否有死結       | 出現在Course Schedule      |

```python
圖論問題
├── 遍歷方式
│   ├── DFS (三色標記)
│   └── BFS (Kahn's algorithm)
│
├── 拓撲排序類 (Course Schedule)
│   ├── DFS + 三色標記
│   └── BFS + 入度表 (Kahn)
│
├── 最短路徑類 (Network Delay Time)
│   ├── Dijkstra
│   ├── Bellman-Ford
│   └── Floyd-Warshall
│
└── 最小生成樹類 (Min Cost)
    ├── Kruskal
    └── Prim
```

## 實際上，新題目只是「組合」而已

舉例：如果題目變化，只是把這些策略**組合**起來：

```python
# 題目變化1: "網路延遲時間" + "有依賴關係"
# = Dijkstra + 拓撲排序

# 題目變化2: "連接所有點" + "有障礙物"
# = MST + BFS

# 題目變化3: "最短路徑" + "可以改變權重"
# = Dijkstra + 優先隊列變形
```

## 真正的「規則」在這裡

你只需要記住**什麼情況用什麼策略**：

```python
# 決策樹
if "最短路徑" in 題目:
    if 權重都是正數:
        用 Dijkstra
    elif 有負權重:
        用 Bellman-Ford
    elif 需要所有點對:
        用 Floyd-Warshall

elif "連接所有點" in 題目:
    if 邊的數量少:
        用 Kruskal
    elif 節點數量少:
        用 Prim

elif "依賴關係" in 題目:
    用 拓撲排序
    # DFS或BFS都可以

elif "環檢測" in 題目:
    用 三色標記法
```

大概還有 3-4 個主要演算法：

1. **Bellman-Ford** (處理負權重的最短路徑)
2. **Floyd-Warshall** (所有點對的最短路徑)
3. **Prim** (另一種MST，適合密集圖)
4. **Union-Find** (你已經在Kruskal用過了！)

```python

會的東西：
✓ 圖的建立 (鄰接表、字典)
✓ DFS遍歷 + 三色標記
✓ BFS遍歷 + 入度表
✓ Dijkstra (最短路徑)
✓ Kruskal (最小生成樹)
✓ Union-Find (並查集)
✓ 優先隊列 (heap)

# 80%的圖論題目都能用這些解決！
```

## 實際面試情況

根據LeetCode統計，**常見圖論題**的分佈：

- 拓撲排序: 15%
- 最短路徑: 25%
- MST: 10%
- DFS/BFS遍歷: 30%
- 其他: 20%

```python
# 當你看到新題目，會這樣思考：

# 1. 這是哪一類？
if "最短時間/距離" in 題目:
    想到 Dijkstra

elif "依賴/順序" in 題目:
    想到 拓撲排序

elif "連接所有點" in 題目:
    想到 MST

# 2. 選遍歷方式
if 需要環檢測 or 路徑記錄:
    用 DFS
elif 需要層次 or 最短步數:
    用 BFS

# 3. 選資料結構
if 需要動態取最小值:
    用 Heap
elif 需要集合操作:
    用 Union-Find
```

## 總結

每道新題目不是全新的，只是：

- 不同策略的**組合**
- 不同資料結構的**搭配**
- 邊界條件的**變化**
