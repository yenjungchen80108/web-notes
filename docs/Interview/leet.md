---
sidebar_position: 1
---

# Leetcode 模板

1. 兩指針 / 滑動窗口
   a. 有序陣列兩數和 / 三數和（Two/Three Sum）

   - 何時用：有序＋找組合和。

   ```js
   let l=0,r=arr.length-1;
   while(l<r){ const sum=arr[l]+arr[r]; if(sum<t) l++; else if(sum>t) r--; else {...}}
   ```

   b. 最長/最短子字串（滑窗）（長度/覆蓋/不重複）

   - 何時用：連續範圍＋條件滿足/最小化。

   ```js
   let l=0,map=new Map(),best=0;
   for(let r=0;r<s.length;r++){
   map.set(s[r], (map.get(s[r])||0)+1);
   while(條件滿足){ best=Math.min(best, r-l+1); /_ 或更新答案 _/ 調整 l 與 map; l++; }
   }
   ```

2. 堆疊 / 單調結構

   a. 有效括號 / Min Stack

   ```js
   const st = [];
   for (const ch of s) {
     if (pair[ch]) st.push(ch);
     else if (st.pop() !== rev[ch]) return false;
   }
   ```

   b. 單調棧（下一個更大/每日溫度）

   ```js
   const st = [],
     ans = Array(n).fill(0);
   for (let i = 0; i < n; i++) {
     while (st.length && A[st.at(-1)] < A[i]) {
       ans[st.pop()] = i;
     }
     st.push(i);
   }
   ```

3. 哈希 / 前綴和

   a. 兩數和（未排序）

   ```js
   const seen = new Map();
   for (let i = 0; i < n; i++) {
     if (seen.has(t - A[i])) return [seen.get(t - A[i]), i];
     seen.set(A[i], i);
   }
   ```

   b. 前綴和 + 哈希（目標和的子陣列數）

   ```js
   let sum = 0,
     cnt = new Map([[0, 1]]),
     ans = 0;
   for (const x of nums) {
     sum += x;
     ans += cnt.get(sum - k) || 0;
     cnt.set(sum, (cnt.get(sum) || 0) + 1);
   }
   ```

4. 區間

   a. 合併區間 / 插入區間

   ```js
   intervals.sort((a, b) => a[0] - b[0]);
   const res = [];
   for (const [s, e] of intervals) {
     if (!res.length || res.at(-1)[1] < s) res.push([s, e]);
     else res.at(-1)[1] = Math.max(res.at(-1)[1], e);
   }
   ```

5. 二分搜尋

   a. 基礎二分（左/右邊界）

   ```js
   let l = 0,
     r = n; // [l,r)
   while (l < r) {
     const m = (l + r) >> 1;
     if (check(m)) r = m;
     else l = m + 1;
   } // 找第一個滿足
   ```

   b. 答案二分（最小可行/最大可行）：把「可行性」寫成 check(mid)。

6. 堆 / 貪心

   a. Top-K / 合併 K 升序串列（小根堆）

   ```js
   // push 起點，pop 最小，補下一個
   ```

   b. 區間選擇 / 會議室（貪心）

   - 排序 + 盡量早結束 or 用最少資源（用最小堆存結束時間）。

   c. Gas Station（你剛做的）

   ```js
   let total = 0,
     tank = 0,
     start = 0;
   for (let i = 0; i < n; i++) {
     const g = gas[i] - cost[i];
     total += g;
     tank += g;
     if (tank < 0) {
       start = i + 1;
       tank = 0;
     }
   }
   return total >= 0 ? start : -1;
   ```

7. 連結串列

   a. 反轉/區間反轉

   ```js
   let prev = null,
     cur = head;
   while (cur) {
     const nxt = cur.next;
     cur.next = prev;
     prev = cur;
     cur = nxt;
   }
   ```

   b. 快慢指針（找中點/環）

   ```js
   let s = head,
     f = head;
   while (f && f.next) {
     s = s.next;
     f = f.next.next;
   }
   ```

8. 樹（DFS/BFS） a. 遍歷（前/中/後）／層序

   ```js
   function dfs(node){ if(!node) return; dfs(node.left); /_ visit _/ dfs(node.right); }
   ```

   b. 路徑和 / 直徑 / 最近共同祖先（LCA）

   - LCA（遞迴回傳在左右子樹是否找到目標）。

9. 回溯（Backtracking）
   a. 子集（Subsets）

   ```js
   const res = [],
     path = [];
   function dfs(i) {
     res.push([...path]);
     for (let k = i; k < n; k++) {
       path.push(A[k]);
       dfs(k + 1);
       path.pop();
     }
   }
   dfs(0);
   ```

   b. 全排列（Permutations）

   ```js
   const res = [],
     path = [],
     used = Array(n).fill(false);
   function dfs() {
     if (path.length === n) {
       res.push([...path]);
       return;
     }
     for (let i = 0; i < n; i++)
       if (!used[i]) {
         used[i] = true;
         path.push(A[i]);
         dfs();
         path.pop();
         used[i] = false;
       }
   }
   dfs();
   ```

   c. 組合總和（Combination Sum / II）（有/無重複、要排序 + 跳重）
   d. 字母矩陣尋路（Word Search）（訪問標記）
   e. N-Queens（列/斜線衝突用 set）

10. 圖（BFS/DFS/拓撲/並查集/最短路）

    a. BFS 最短步數（無權圖）

    ```js
    const q = [[src, 0]],
      seen = new Set([src]);
    while (q.length) {
      const [u, d] = q.shift();
      for (const v of G[u])
        if (!seen.has(v)) {
          seen.add(v);
          q.push([v, d + 1]);
        }
    }
    ```

    b. 拓撲排序（Kahn 或 DFS）

    ```js
    const indeg=Array(n).fill(0); // 建圖+入度
    const q=[]; for(i) if(!indeg[i]) q.push(i);
    while(q.length){ const u=q.shift(); for(v of G[u]) if(--indeg[v]==0) q.push(v); }
    ```

    c. Union-Find（連通塊 / 冗餘邊）

    ```js
    const p=Array(n).fill(0).map((\_,i)=>i);
    const find=x=>p[x]==x?x:(p[x]=find(p[x]));
    const uni=(a,b)=>{ a=find(a); b=find(b); if(a!=b) p[a]=b; }
    ```

11. 動態規劃（1D/2D）
    a. 爬樓梯 / 打家劫舍（線性 DP）

    ```js
    let a = 0,
      b = 1;
    for (let i = 0; i < n; i++) {
      [a, b] = [b, a + b];
    } // Fib 模板
    ```

    b. 0/1 背包 / Coin Change

    ```js
    const dp = Array(target + 1).fill(Infinity);
    dp[0] = 0;
    for (const c of coins) {
      for (let x = c; x <= target; x++) dp[x] = Math.min(dp[x], dp[x - c] + 1);
    }
    ```

c. LIS（最長遞增子序列）— patience sorting

    ```js
    const piles = [];
    for (const x of A) {
    let l = 0,
        r = piles.length;
    while (l < r) {
        const m = (l + r) >> 1;
        if (piles[m] >= x) r = m;
        else l = m + 1;
    }
    piles[l] = x;
    }
    ```

    d.	LCS / 編輯距離（2D DP）
    ```js
    const dp = Array(m + 1)
    .fill()
    .map(() => Array(n + 1).fill(0)); // LCS

    // 或 Edit Distance：插入/刪除/替換 取最小
    ```

12. 回文相關 DP

    a. 最長回文子串（中心擴展 / DP）

    ```js
    function expand(l, r) {
      while (l >= 0 && r < n && s[l] == s[r]) {
        l--;
        r++;
      }
      return [l + 1, r - 1];
    }
    ```

    b. 回文分割（最少切割 / 所有分割）

    - 131：DFS + isPalindrome 子字串檢查（可預處理 pal[i][j]）。

    ```js
    function isPalindrome(s) {
      let l = 0,
        r = s.length - 1;
      while (l < r) {
        if (s[l] !== s[r]) return false;
        l++;
        r--;
      }
      return true;
    }
    ```

使用建議（你說的「背 20–30 模板」怎麼背）

- 每類挑 2~3 題代表作，做成你自己的模板（函式骨架 + 常見變體怎麼改）。
- 練到「看到題就能判斷題型」：
- 連續子陣列 → 滑窗 / 前綴和
- 最短步數 → BFS
- “是否能走完一圈”→ 貪心（Gas Station）
- 切割字串 → DFS backtracking
- “最小/最大可行解” → 答案二分
- 設計資料結構 → Map/Heap/LinkedList/Union-Find/Trie

```

```
