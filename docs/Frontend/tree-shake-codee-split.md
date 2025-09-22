---
sidebar_position: 11
---

# Tree Shaking and Code Splitting in Webpack

https://blog.logrocket.com/tree-shaking-and-code-splitting-in-webpack/

## Core Concept 1: Tree Shaking（樹搖）

- 定義：Tree Shaking 又稱「死碼消除（Dead Code Elimination）」，意指移除專案中沒被使用的程式碼，讓最終打包的程式碼更輕量 ￼。
- Why：減少下載體積、提升載入速度、改善使用者體驗。
- Webpack 支援：
  - 更好地支援 ES Modules（import/export），因為可以靜態分析引用。
  - 比較難支援 CommonJS、AMD、UMD，因為它們可能動態載入/執行，Webpack 無法靜態預測 ￼。
- 使用方式：
  - 避免動態載入（例如 App[variable]）。
  - 使用 production 模式和設定 usedExports、sideEffects: false 提供最佳效果 ￼。

## Core Concept 2: Code Splitting（程式碼分割）

- 定義：將程式拆分成多個可按需載入的「chunk」，減少初始 bundle 的大小 ￼。
- 為什麼要做？
  - 初始載入更快，使用者體驗更好。
  - 次要或條件功能只在需要時才載入，避免浪費資源。
- 如何實作：
  - 使用多個 entry points 手動切分。
  - 使用 SplitChunksPlugin 去重複模組。
  - 實作動態 import() 在程式邏輯運行時載入。

## Summary and Analogy

你可以想像專案像一棵「知識樹」：

- Tree Shaking 就是把沒用到的枝幹剪掉／落葉掃掉，只留下需要的部分。
- Code Splitting 則是把整棵樹拆成小樁，在需要時再植回，用更少東西先搭建「樹頭」，其他枝幹按需接來，整個體積變輕，生長更快。

⸻

最實用建議

- 如果想快速啟用這兩個功能：
  - 確保使用 ES Modules。
  - 在 webpack.config.js 設定：

```js
mode: 'production',
optimization: {
usedExports: true, // 啟用 treeshaking
}
```

- 不要用動態 require，儘量用 import() 分割 code。
- （可選）在 package.json 加上 "sideEffects": false 讓 Webpack 更積極剔除無效程式碼。
