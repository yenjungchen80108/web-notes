---
sidebar_position: 1
---

# Djikstra Algorithm

## Dijkstra 演算法概念

Dijkstra 是一種解決「單源最短路徑」問題的貪心演算法，適用於非負權重的有向或無向圖。

給定一個起點 s，它能在 O(V^2)（或搭配堆、鄰接表時 O(E\log V)）的時間內，算出從 s 到圖中所有其他點的最短距離。

演算法步驟（以鄰接表＋陣列選最小值實作）

1. 初始化

- dist[u]：從起點 s 到 u 的目前已知最短距離，初始為 ∞。
- visited[u]：標記是否已「確定最短路徑」的節點，初始都為 false。
- 設 dist[s] = 0。

2. 重複 V 次

a. 從所有 visited=false 的節點中，找出 dist[u] 最小的那個節點 u，並標為已訪問（visited[u]=true）。

b. 以 u 為中繼，檢查它所有鄰居 v：

```js
if visited[v] == false  且
dist[u] + weight(u→v) < dist[v]：
→ 更新 dist[v] = dist[u] + weight(u→v)
```

3. 結束後，dist[] 即為從起點 s 到各頂點的最短路徑長度；若要重建路徑，可在鬆弛（relax）時同時記錄前驅 prev[v] = u。

> 關鍵重點：
> 「放鬆邊」＝「試圖用剛剛確定的 u 去更新它所有鄰居的最短路徑，如果更短就更新」。這個動作讓演算法一步步把最短路徑「擴散」出去，最終把所有能到達的城市的正確最短距離都算出來。

## 時間／空間複雜度

- 時間複雜度
- 陣列＋線性搜尋最小值：O(V^2 + E)\approx O(V^2)
- 二元堆＋鄰接表：O((V+E)\log V)\approx O(E\log V)
- 空間複雜度：O(V + E) 用於儲存鄰接表，以及 O(V) 的 dist 和 visited 陣列。

## 簡易 JavaScript 範例

```js
function dijkstraWithPath(graph, start) {
  const dist = {};
  const visited = {};
  const prev = {}; // ← 紀錄前驅
  const nodes = Object.keys(graph);

  // 初始化
  nodes.forEach((u) => {
    dist[u] = Infinity;
    visited[u] = false;
    prev[u] = null; // 起初不知從哪裡來
  });
  dist[start] = 0;

  // 主迴圈
  for (let i = 0; i < nodes.length; i++) {
    // 找當前未訪問且 dist 最小的節點 u
    let u = null,
      minD = Infinity;
    for (const v of nodes) {
      if (!visited[v] && dist[v] < minD) {
        minD = dist[v];
        u = v;
      }
    }
    if (u === null) break;
    visited[u] = true;

    // 放鬆從 u 出發的每條邊
    for (const { to: v, w } of graph[u]) {
      if (!visited[v] && dist[u] + w < dist[v]) {
        dist[v] = dist[u] + w;
        prev[v] = u; // ← 在這裡記錄：v 最短路徑的前一步是 u
      }
    }
  }

  return { dist, prev };
}

// 回溯路徑的輔助函式
function buildPath(prev, target) {
  const path = [];
  let u = target;
  while (u !== null) {
    path.push(u);
    u = prev[u];
  }
  return path.reverse(); // 反轉就從起點到目標
}

// 範例
const graph = {
  A: [
    { to: "B", w: 5 },
    { to: "C", w: 2 },
  ],
  B: [
    { to: "C", w: 1 },
    { to: "D", w: 3 },
  ],
  C: [
    { to: "B", w: 3 },
    { to: "D", w: 7 },
    { to: "E", w: 4 },
  ],
  D: [{ to: "E", w: 2 }],
  E: [],
};

const { dist, prev } = dijkstraWithPath(graph, "A");
console.log(dist); // 最短距離
console.log(buildPath(prev, "E")); // 比如：['A','C','E']
```

這段程式碼採用最直觀的陣列＋線性搜尋最小值，易讀易懂，適合中小型圖的教學或示範。
若要進一步優化，可把「挑最小 dist」改成最小堆，性能可提升到 O(E\log V)。
