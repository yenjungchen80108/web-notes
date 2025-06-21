---
sidebar_position: 2
---

# AVL Tree

## 平衡二元搜尋樹的定義

平衡二元搜尋樹（AVL Tree）是一種自平衡的二元搜尋樹，它滿足以下條件：

1. 每個節點的左子樹和右子樹的高度差不能超過 1。
2. 每個節點的左子樹和右子樹都是平衡二元搜尋樹。

AVL Tree 的核心概念：保持平衡，也就是每個節點的 平衡因子（Balance Factor）為 -1、0 或 1。

⸻

✅ AVL Tree 最基本 JS 實作（插入 + 左右旋轉）

```js
class Node {
  constructor(value) {
    this.value = value;
    this.height = 1; // 初始高度設為1（不是0）
    this.left = null;
    this.right = null;
  }
}

class AVLTree {
  constructor() {
    this.root = null;
  }

  // 獲取節點高度
  getHeight(node) {
    if (!node) return 0;
    return node.height;
  }

  // 計算平衡因子
  getBalance(node) {
    if (!node) return 0;
    return this.getHeight(node.left) - this.getHeight(node.right);
  }

  // 右旋轉（Left heavy）
  rotateRight(y) {
    const x = y.left;
    const T2 = x.right;

    x.right = y;
    y.left = T2;

    // 更新高度
    y.height = 1 + Math.max(this.getHeight(y.left), this.getHeight(y.right));
    x.height = 1 + Math.max(this.getHeight(x.left), this.getHeight(x.right));

    return x; // 新的根
  }

  // 左旋轉（Right heavy）
  rotateLeft(x) {
    const y = x.right;
    const T2 = y.left;

    y.left = x;
    x.right = T2;

    // 更新高度
    x.height = 1 + Math.max(this.getHeight(x.left), this.getHeight(x.right));
    y.height = 1 + Math.max(this.getHeight(y.left), this.getHeight(y.right));

    return y; // 新的根
  }

  // 插入節點（重點）
  insert(node, value) {
    if (!node) return new Node(value);

    if (value < node.value) {
      node.left = this.insert(node.left, value);
    } else if (value > node.value) {
      node.right = this.insert(node.right, value);
    } else {
      return node; // 相同值不處理
    }

    // 更新高度
    node.height =
      1 + Math.max(this.getHeight(node.left), this.getHeight(node.right));

    // 平衡判斷
    const balance = this.getBalance(node);

    // LL case 插入在左子節點的左邊
    if (balance > 1 && value < node.left.value) {
      return this.rotateRight(node);
    }

    // RR case 插入在右子節點的右邊
    if (balance < -1 && value > node.right.value) {
      return this.rotateLeft(node);
    }

    // LR case 插入在左子節點的右邊
    if (balance > 1 && value > node.left.value) {
      node.left = this.rotateLeft(node.left);
      return this.rotateRight(node);
    }

    // RL case 插入在右子節點的左邊
    if (balance < -1 && value < node.right.value) {
      node.right = this.rotateRight(node.right);
      return this.rotateLeft(node);
    }

    return node;
  }

  insertValue(value) {
    this.root = this.insert(this.root, value);
  }

  // 中序遍歷（用來 debug 結果）
  inOrder(node = this.root) {
    if (node) {
      this.inOrder(node.left);
      console.log(node.value);
      this.inOrder(node.right);
    }
  }
}

// 🧪 測試
const tree = new AVLTree();
tree.insertValue(10);
tree.insertValue(20);
tree.insertValue(30); // 會觸發 RR 旋轉
tree.insertValue(40);
tree.insertValue(50); // 再次觸發 RR
tree.insertValue(25); // RL case

console.log("🟢 中序遍歷:");
tree.inOrder(); // 印出排序後的結果
```

⸻

🧠 對應的旋轉四種情況總結

| Case | 條件說明             | 修復方式       |
| ---- | -------------------- | -------------- |
| LL   | 插入在左子節點的左邊 | 右旋轉         |
| RR   | 插入在右子節點的右邊 | 左旋轉         |
| LR   | 插入在左子節點的右邊 | 先左轉，再右轉 |
| RL   | 插入在右子節點的左邊 | 先右轉，再左轉 |

⸻

📌 小重點提醒
• AVL 是一種「自平衡」二元搜尋樹，插入和刪除都會在需要時進行旋轉
• 插入時需要回溯（recursively）去調整每個節點的平衡因子並判斷是否需要旋轉
• 本例只實作了插入 insert，你也可以進一步加上 remove 和 search 功能

⸻

如需我幫你把這個 AVL Tree 加上視覺化（例如 HTML + SVG 畫出來）或補充刪除（delete）功能，隨時告訴我！
