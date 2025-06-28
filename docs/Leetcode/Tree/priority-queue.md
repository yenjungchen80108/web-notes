---
sidebar_position: 5
---

# Priority Queue

## Priority Queue 是什麼？

Priority Queue（優先佇列）是一種「依照優先順序排隊」的資料結構，不同於一般 FIFO 的 queue，誰的優先級高，誰就先出列。

它的底層實作，通常是 Min Heap 或 Max Heap，來達到高效率的插入與取出。

⸻

🧠 重點特性

| 操作                | 時間複雜度 |
| ------------------- | ---------- |
| 插入元素            | O(log n)   |
| 取得最高/最低優先級 | O(1)       |
| 移除最高/最低元素   | O(log n)   |

⸻

🧰 常見用途

1. 任務排程器（如 OS 的執行緒排程）
2. Dijkstra 最短路徑演算法
3. A 路徑規劃
4. LeetCode 題：Top K 頻率元素、合併 K 個排序鏈表

⸻

🧪 JavaScript 模擬 Priority Queue

JS 原生沒有 PriorityQueue，我們用 MinHeap 模擬一個簡單的範例：

```js
class PriorityQueue {
  constructor() {
    this.heap = [];
  }

  enqueue(value, priority) {
    this.heap.push({ value, priority });
    this.bubbleUp();
  }

  dequeue() {
    const top = this.heap[0];
    const end = this.heap.pop();
    if (this.heap.length > 0) {
      this.heap[0] = end;
      this.bubbleDown();
    }
    return top;
  }

  bubbleUp() {
    let index = this.heap.length - 1;
    const element = this.heap[index];

    while (index > 0) {
      const parentIndex = Math.floor((index - 1) / 2);
      const parent = this.heap[parentIndex];

      if (element.priority >= parent.priority) break;

      this.heap[parentIndex] = element;
      this.heap[index] = parent;
      index = parentIndex;
    }
  }

  bubbleDown() {
    let index = 0;
    const length = this.heap.length;
    const element = this.heap[0];

    while (true) {
      let left = 2 * index + 1;
      let right = 2 * index + 2;
      let swap = null;

      if (left < length && this.heap[left].priority < element.priority) {
        swap = left;
      }
      if (
        right < length &&
        this.heap[right].priority <
          (this.heap[swap]?.priority ?? element.priority)
      ) {
        swap = right;
      }

      if (swap === null) break;

      this.heap[index] = this.heap[swap];
      this.heap[swap] = element;
      index = swap;
    }
  }
}
```

使用方式

```js
const pq = new PriorityQueue();
pq.enqueue("eat", 5); // 優先級5
pq.enqueue("code", 1); // 優先級1
pq.enqueue("sleep", 3); // 優先級3

console.log(pq.dequeue()); // { value: "code", priority: 1 }
```

⸻

✅ 總結一句話

Priority Queue 是一個「有排序能力的 Queue」，背後通常是用 Heap 實作，並根據 priority 來決定誰先出列。
