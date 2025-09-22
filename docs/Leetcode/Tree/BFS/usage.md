---
sidebar_position: 1
---

# BFS 用法

BFS Queue 放什麼？

A. 放「單一節點」：q.push(node)

情境：只需要逐節點處理或修改，不用成對比較、也不用額外資訊。
代表題型 / 任務

- 反轉二元樹（invert tree）
- 層序走訪（level order）/ 收集每層節點
- 計算節點總數、累加值
- 廣度搜尋找值是否存在

範例

```js
const q = [root];
while (q.length) {
  const node = q.shift();
  [node.left, node.right] = [node.right, node.left]; // 例如：反轉
  if (node.left) q.push(node.left);
  if (node.right) q.push(node.right);
}
```

⸻

B. 放「成對節點」：q.push([a, b])

情境：需要同時比較兩個對應位置的節點（鏡像、等價、合併）。
代表題型 / 任務

- 判斷對稱樹（symmetric tree）
- 比較兩棵樹是否相同（same tree）
- 合併二元樹（merge trees，雖然也可 DFS）

範例（對稱樹）

```js
const q = [[root.left, root.right]];
while (q.length) {
  const [a, b] = q.shift();
  if (!a && !b) continue;
  if (!a || !b || a.val !== b.val) return false;
  q.push([a.left, b.right]);
  q.push([a.right, b.left]);
}
return true;
```

⸻

C. 放「節點 + 附帶資訊」：q.push([node, info])

情境：除了節點，還要帶著狀態一起走（深度、距離、座標、路徑狀態）。
代表題型 / 任務

- 最短路徑長度（min depth / grid BFS）
- 層級標記（需要回傳每層或深度）
- 網格題（迷宮、腐爛橘子、島嶼數量 with visited）
- 圖論最短路徑（帶距離）
- 二元樹的垂直/水平序（帶座標）

範例（最小深度）

```js
const q = [[root, 1]];
while (q.length) {
  const [node, depth] = q.shift();
  if (!node.left && !node.right) return depth;
  if (node.left) q.push([node.left, depth + 1]);
  if (node.right) q.push([node.right, depth + 1]);
}
```

範例（網格 BFS：節點=座標 + 距離）

```js
const q = [[sx, sy, 0]];
const seen = new Set([`${sx},${sy}`]);
while (q.length) {
  const [x, y, d] = q.shift();
  for (const [dx, dy] of [
    [1, 0],
    [-1, 0],
    [0, 1],
    [0, -1],
  ]) {
    const nx = x + dx,
      ny = y + dy;
    const key = `${nx},${ny}`;
    if (/_ 合法且未訪問 _/ && !seen.has(key)) {
      if (/_ 抵達目標 _/) return d + 1;
      seen.add(key);
      q.push([nx, ny, d + 1]);
    }
  }
}
```

⸻

快速判斷術

- 只改當前節點？ → node
- 要比對兩側是否對應？ → [a, b]
- 要帶著深度/距離/座標/狀態？ → [node, info]
