---
sidebar_position: 6
---

# Trie

Trie（發音像「try」，又稱 前綴樹 / 字典樹）是一種用來儲存 字串集合（通常是單詞） 的樹狀結構，特別適合處理「前綴查詢」的問題，比如：

- ✅ 自動完成（autocomplete）
- ✅ 拼字檢查（spell checking）
- ✅ 搜尋某個前綴開頭的所有單字（startsWith）
- ✅ 搜尋字典中是否存在某個完整單字（search）

⸻

🔧 Trie 的基本結構

每個 Trie 節點代表一個字元，整體結構長這樣：

```
           root
          /   \
        a       b
       / \       \
      p   t       a
     /     \       \
    p       e       t
   /         \
  l           r
 /             \
e               s
```

這個 Trie 包含的單字有：apple, apt, bat, bats, ate, art

⸻

🧱 Trie 節點的設計（以 JavaScript 為例）

```js
class TrieNode {
  constructor() {
    this.children = {}; // 每個子節點都是一個字母對應的 TrieNode
    this.isEnd = false; // 標記是否為一個完整單字的結尾
  }
}
```

⸻

🏗 Trie 樹的核心操作

1️⃣ 插入單字 insert(word)

```js
insert(word) {
  let node = this.root;
  for (let char of word) {
    if (!node.children[char]) {
      node.children[char] = new TrieNode();
    }
    node = node.children[char];
  }
  node.isEnd = true;
}
```

2️⃣ 搜尋單字是否存在 search(word)

```js
search(word) {
  let node = this.root;
  for (let char of word) {
    if (!node.children[char]) return false;
    node = node.children[char];
  }
  return node.isEnd;  // 要確保是完整的單字
}
```

3️⃣ 查詢是否有這個前綴開頭的字 startsWith(prefix)

```js
startsWith(prefix) {
  let node = this.root;
  for (let char of prefix) {
    if (!node.children[char]) return false;
    node = node.children[char];
  }
  return true;
}
```

⸻

🧠 為什麼 Trie 很有用？

| 功能         | 說明                                                          |
| ------------ | ------------------------------------------------------------- |
| 快速查字     | 時間複雜度為 O(k)，k 為字長，與字典大小無關                   |
| 支援前綴查詢 | 對於像「輸入 ‘app’ 自動補出 apple, application」的功能很強    |
| 空間可壓縮   | 雖然節點數多，但可用 Map, Array, 位元壓縮 等方式優化          |
| 可擴充       | 可進一步變成 壓縮 Trie（Radix Tree） 或 Suffix Trie（後綴樹） |

⸻

📦 小結

| 操作       | 時間複雜度 |
| ---------- | ---------- |
| 插入       | O(k)       |
| 搜尋       | O(k)       |
| startsWith | O(k)       |

⚠️ 注意：Trie 是用空間換取查詢效率，會佔用較多記憶體。

⸻

這裡是一個完整的 JavaScript Trie 實作，包含常見的三個方法：insert、search 和 startsWith，非常適合理解 Trie 的基本運作：

⸻

✅ JavaScript Trie 完整實作

```js
class TrieNode {
  constructor() {
    this.children = {}; // 用物件儲存每個子節點 (key 是字母)
    this.isEnd = false; // 是否為一個完整單字的結尾
  }
}

class Trie {
  constructor() {
    this.root = new TrieNode();
  }

  // 插入單字
  insert(word) {
    let node = this.root;
    for (let char of word) {
      if (!node.children[char]) {
        node.children[char] = new TrieNode();
      }
      node = node.children[char];
    }
    node.isEnd = true;
  }

  // 搜尋完整單字是否存在
  search(word) {
    let node = this._searchPrefix(word);
    return node !== null && node.isEnd === true;
  }

  // 檢查是否存在某個前綴開頭的單字
  startsWith(prefix) {
    return this._searchPrefix(prefix) !== null;
  }

  // 私有方法：找到 prefix 結尾的節點
  _searchPrefix(prefix) {
    let node = this.root;
    for (let char of prefix) {
      if (!node.children[char]) {
        return null;
      }
      node = node.children[char];
    }
    return node;
  }
}
```

⸻

🧪 使用範例

```js
const trie = new Trie();

trie.insert("apple");
console.log(trie.search("apple")); // true
console.log(trie.search("app")); // false
console.log(trie.startsWith("app")); // true

trie.insert("app");
console.log(trie.search("app")); // true
```

⸻

📌 小提醒
• 使用 startsWith() 不會檢查是否為完整單字，只檢查是否有該前綴。
• Trie 是 用空間換時間 的結構，適合大量單字的搜尋系統（像搜尋引擎、拼音輸入法、搜尋建議）。

⸻
