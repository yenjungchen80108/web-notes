1️⃣ 基礎概念

Webpack 是什麼

- 模組打包工具（Module Bundler）
- 將多種資源（JS、CSS、圖片等）視為模組，透過 loader / plugin 進行轉換與打包

四個核心概念

1. Entry

- 打包入口
- 可設定多入口（多頁面應用）

> entry: './src/index.js'

2. Output

- 打包輸出位置與檔名

```js
output: {
  path: path.resolve(__dirname, 'dist'),
  filename: '[name].[contenthash].js'
}
```

3. Loader

- 處理非 JS 檔案，例如 Babel Loader、CSS Loader
- 轉換成 Webpack 可理解的模組

4. Plugin

- 負責功能擴展，例如壓縮、環境變數注入、HTML 模板生成等
- 常見：HtmlWebpackPlugin、MiniCssExtractPlugin

⸻

2️⃣ 進階配置與原理

模組解析

- 支援 import、require、url() 等
- resolve 可配置別名（alias）、副檔名省略

Webpack Dev Server (WDS)

- 提供熱更新（HMR）
- 預設存在於記憶體，不會真的寫入 dist

Tree Shaking

- 移除未使用的程式碼（ESM 靜態分析）
- 必須 mode: 'production' 或手動開啟 usedExports: true

Code Splitting

- 分包策略，減少首屏載入時間
- 三種方式：
  1. 多入口配置
  2. import() 動態引入
  3. SplitChunksPlugin 公共代碼抽離

Source Map

- 開發除錯用，可選擇不同級刄：
  - eval-source-map（快，較大）
  - source-map（完整映射）
  - cheap-module-source-map（快，精度低）

Hash 類型

- hash：整體專案改變就改
- chunkhash：單檔案改變才改
- contenthash：檔案內容變才改（最佳緩存）

⸻

3️⃣ 性能優化考點

打包速度

- 使用 cache: true（Webpack 5 預設啟用）
- 開啟多執行緒（thread-loader）
- babel-loader 搭配 cacheDirectory: true
- 排除第三方庫（exclude: /node_modules/）

打包體積

- Tree Shaking
- Code Splitting
- CDN 外掛大型庫（React、Vue）
- 使用 MiniCssExtractPlugin 分離 CSS
- 圖片壓縮（image-webpack-loader）

加載優化

- 懶加載與預加載（import() + /_ webpackPrefetch _/）
- 瀏覽器快取（contenthash）
- Gzip / Brotli 壓縮

⸻

4️⃣ 常見面試題

Q1: Webpack 與 Vite 的差異？

- Webpack：bundle-based（打包後才能運行），生態成熟
- Vite：基於 ES Modules 即時載入，開發啟動快，適合大型專案的開發階段，但最終仍需打包

Q2: Loader 與 Plugin 的區別？

- Loader：轉換模組內容（檔案級別）
- Plugin：擴展功能（整體編譯流程級別）

Q3: 如何優化 Webpack 構建速度？

- 多執行緒（thread-loader）
- cache
- exclude node_modules
- 使用 alias 縮短路徑解析

Q4: 如何減少打包體積？

- Tree Shaking
- Code Splitting
- CDN 引入大庫
- 使用 gzip

Q5: 為什麼要用 contenthash？

- 避免不必要的快取失效，確保只更新變動檔案

Q6: 如何實現按需加載？

- 使用 import() 動態引入
- 使用 require.ensure
- 使用 webpackInclude/webpackExclude

Q7: 如何實現多入口打包？

- 使用 entry 配置多個入口
- 使用 glob 匹配多個入口
