---
sidebar_position: 3
---

# Red-Black Tree

## 紅黑樹的定義

紅黑樹（Red-Black Tree）是一種自平衡的二元搜尋樹，它滿足以下條件：

1. 每個節點是紅色或黑色。
2. 根節點是黑色。
3. 每個葉子節點（NIL 節點）是黑色。
4. 新插入的節點是紅色。
5. 每條從 root 到 null 的路徑，黑節點數量都要一樣

📏 1. 為什麼需要平衡？

假設你把 1~10 依序插入普通 BST，它會長這樣：

```
1
 \
  2
   \
    3
     \
     ...
       \
        10
```

這根本就是 Linked List，查找要 O(n)，太慢。
所以我們需要「平衡」讓它變回像這樣：

```
        5
      /   \
     3     8
    / \   / \
   2  4  6   9
  /           \
 1            10
```

紅黑樹能自動幫你做到這點。

⸻

🧩 2. 插入時會發生什麼事？

假設你插入一個紅色節點，違反了「紅色不能連紅色」這條規則，怎麼辦？

就會啟動：
• 重新塗色（recolor）
• 或是 旋轉（rotate），讓樹重新恢復平衡

紅黑樹不像 AVL Tree 插入就要旋轉，只有在違反規則時才會調整，調整次數通常比 AVL Tree 少。

⸻

🔁 3. 一個簡單例子（插入 + 自動修正）

假設你插入三個數字：10, 20, 30

紅黑樹第一步長這樣：

10(黑)

插入 20（紅）：OK，沒有違規

```
    10(黑)
       \
       20(紅)
```

插入 30（紅）→ 現在出現紅紅（20 是紅，30 也是紅）→ 違規 ❌

這時會觸發旋轉，把它變成：

```
    20(黑)
   /    \
10(紅) 30(紅)
```

現在就平衡了 ✅

⸻

🧠 4. 紅黑樹 vs AVL Tree：簡單比較

| 比較點      | AVL Tree                    | Red-Black Tree             |
| ----------- | --------------------------- | -------------------------- |
| 平衡性      | 非常嚴格（高）              | 中等（比較寬鬆）           |
| 查找效率    | 更快（因為更平衡）          | 稍慢（但仍是 O(log n)）    |
| 插入 / 刪除 | 調整次數多，操作較重        | 調整少，操作較快           |
| 使用場景    | 寫入少但查詢多              | 寫入查詢都頻繁的場景       |
| 常見應用    | DB index (如 SQLite B+Tree) | Java TreeMap, Linux Kernel |

```js
class Node {
  constructor(value, color = "RED") {
    this.value = value;
    this.color = color;
    this.left = null;
    this.right = null;
    this.parent = null;
  }
}

class RedBlackTree {
  constructor() {
    this.root = null;
  }

  insert(value) {
    const newNode = new Node(value);
    if (!this.root) {
      newNode.color = "BLACK"; // 規則2：root 為黑色
      this.root = newNode;
      return;
    }

    let current = this.root;
    let parent = null;
    while (current) {
      parent = current;
      if (value < current.value) {
        current = current.left;
      } else {
        current = current.right;
      }
    }

    newNode.parent = parent;
    if (value < parent.value) {
      parent.left = newNode;
    } else {
      parent.right = newNode;
    }

    this.fixInsert(newNode);
  }

  fixInsert(node) {
    // 真正紅黑平衡修復邏輯（需寫 rotate / uncle check）
    console.log(`Fixing balance for node: ${node.value}`);
  }
}

const tree = new RedBlackTree();
tree.insert(10);
tree.insert(20);
tree.insert(15);
```

⸻

```js
fixInsert(z) {
  while (z.parent && z.parent.color === 'RED') {
    const grandparent = z.parent.parent;
    if (z.parent === grandparent.left) {
      const uncle = grandparent.right;
      if (uncle && uncle.color === 'RED') {
        // Case 1: Uncle is RED → recolor
        z.parent.color = 'BLACK';
        uncle.color = 'BLACK';
        grandparent.color = 'RED';
        z = grandparent;
      } else {
        if (z === z.parent.right) {
          // Case 2: z is right child → rotate left
          z = z.parent;
          this.rotateLeft(z);
        }
        // Case 3: z is left child → rotate right
        z.parent.color = 'BLACK';
        grandparent.color = 'RED';
        this.rotateRight(grandparent);
      }
    } else {
      // Same as above but mirror left/right
      const uncle = grandparent.left;
      if (uncle && uncle.color === 'RED') {
        z.parent.color = 'BLACK';
        uncle.color = 'BLACK';
        grandparent.color = 'RED';
        z = grandparent;
      } else {
        if (z === z.parent.left) {
          z = z.parent;
          this.rotateRight(z);
        }
        z.parent.color = 'BLACK';
        grandparent.color = 'RED';
        this.rotateLeft(grandparent);
      }
    }
  }

  this.root.color = 'BLACK'; // 根節點永遠為黑
}
```

⸻

```js
rotateLeft(x) {
  const y = x.right;
  x.right = y.left;
  if (y.left) y.left.parent = x;
  y.parent = x.parent;

  if (!x.parent) {
    this.root = y;
  } else if (x === x.parent.left) {
    x.parent.left = y;
  } else {
    x.parent.right = y;
  }

  y.left = x;
  x.parent = y;
}

rotateRight(y) {
  const x = y.left;
  y.left = x.right;
  if (x.right) x.right.parent = y;
  x.parent = y.parent;

  if (!y.parent) {
    this.root = x;
  } else if (y === y.parent.left) {
    y.parent.left = x;
  } else {
    y.parent.right = x;
  }

  x.right = y;
  y.parent = x;
}
```
