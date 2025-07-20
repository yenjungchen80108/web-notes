---
sidebar_position: 2
---

# Tarjan Algorithm

割點（Articulation Point）的定義：

在一個無向連通圖中，如果去掉某個頂點（以及與之相連的所有邊）後，圖的連通分量數目增加，那麼這個頂點就稱為「割點」或「關節點」。

割點就像圖中的橋樑：一旦移除，就會讓圖斷開。

## 核心概念

我們用一次深度優先搜尋（DFS）同時維護兩個數值：

1. disc[u]：頂點 u 在 DFS 中被「發現」的時間戳（第幾步）。
2. low[u]：從 u 出發，沿著若干條樹邊（DFS 樹邊）再最多一條回邊，能夠回溯到的最早被發現頂點的時間戳。

直觀理解：

- 如果某子樹根節點 v 的 low[v] 大於等於它的父節點 u 的 disc[u]，代表從 v 及其後代都無法繞過 u 回到更早的點，只能經由 u，因此 u 就是割點。

⸻

## 演算法步驟

1. 初始化

- 令 time = 0。
- 對所有頂點 u：

disc[u] = -1 // 尚未訪問
low[u] = -1
parent[u] = NIL // 父節點
isAP[u] = false // 是否為割點

2. 對每個未訪問過的頂點 u 執行 DFS

```js
if (disc[u] == -1) DFS(u);
```

3. DFS(u) 過程

```js
disc[u] = low[u] = ++time
childCount = 0

for 每個鄰居 v of u:
if (disc[v] == -1): // v 尚未訪問
parent[v] = u
childCount++
DFS(v)

    // 回溯時更新 low[u]
    low[u] = min(low[u], low[v])

    // 根節點特殊情況：若根節點有 2 個以上子節點，則它是割點
    if (parent[u] == NIL and childCount > 1):
      isAP[u] = true

    // 非根節點情況：若 v 的子樹無法透過回邊回到 u 之前，則 u 是割點
    if (parent[u] != NIL and low[v] >= disc[u]):
      isAP[u] = true

else if (v != parent[u]): // 已訪問過且不是從父節點回來的「回邊」
low[u] = min(low[u], disc[v])
```

4. 結果

所有 isAP[u] == true 的頂點即為割點。

⸻

## 何時會用到這個？

- 網路可靠度分析：判斷哪些關鍵路由器或伺服器一旦失效，就會造成網路分割。
- 基礎設施／電力系統：在設計電網、交通網等時，找出會造成全域斷鏈的樞紐節點，以便做冗餘備援。
- 社群網路分析：找出哪些用戶是社群結構中的「樞紐」，他們的移除會顯著降低資訊傳播效率。
- 程式依賴圖：在大型軟體系統中，定位哪些模組或函式庫是關鍵依賴，一旦移除會造成整個系統無法正常運作。

透過割點分析，可以在各種需要提升容錯性、建立備援或理解結構脆弱性的場景下，精準識別出最危險或最關鍵的節點。

reference: [Tarjan Algorithm](https://youtu.be/jFZsDDB0-vo?si=N6STUQiGN4GdaB3P)

[Leetcode Link](https://leetcode.com/problems/critical-connections-in-a-network/)
