1. React 為何需要 key？用 index 當 key 的坑是什麼？

2. useEffect 相依陣列怎麼判定？什麼是 stale closure？三種解法各是什麼？

3. React.memo/useMemo/useCallback 什麼時候有效？什麼時候是過度優化？

4. Suspense for Data Fetching 的基本概念；Error Boundary 角色？

5. 受控 vs 非受控元件差異，表單大量輸入時的效能策略？

6. 列表虛擬化的取捨（react-window / react-virtuoso）；如何處理「往上載入不抖動」？

7. 全域狀態方案比較：Context（selector pattern）、Redux、Zustand 的適用面。

8. SSR/Hydration 的常見錯誤與排查（例如 hydration mismatch）。

---

**1. React 為何需要 key？為何別用 index？**

- key 提供穩定的項目身分，讓 Reconciliation 把「舊虛擬節點」對到「同一個實體項目」：插入/刪除/重排時，React 才能重用正確的 DOM 與 state，而不是錯配。
- 用 index 當 key 在重排或插入中間時會導致錯位（A 的 DOM/內部狀態跑到 B）、多餘重渲染，只有純 append、永不重排時才勉強可用。

- 不是因為「index 可能一樣」，而是相對位置會變。
- 不用提到「Fiber cancellation」；核心是「穩定身分」與「避免錯配」。

**2. useEffect 相依陣列怎麼判定？什麼是 stale closure？解法？**

- 相依陣列規則：把 Effect 內用到的外部可變值（props、state、在外層定義的變數/函式）全部放進去。違反就可能讀到舊值。
- Stale closure：Effect/回呼捕捉了當時的值與函式，之後渲染更新但它仍用舊快照。
- 三種常見解法：
  1. 把用到的值/函式放進 deps，必要時用 useCallback 穩定函式身分。
  2. 用 函式型 setState 或 useReducer，把「舊值 → 新值」邏輯放到 reducer，避免依賴外部值。
  3. 用 useRef/事件回呼模式（或 useEvent in React 19）保存最新值給回呼讀取，但不觸發重渲染。

```js
function Comp({ query }) {
  const [data, setData] = useState(null);
  useEffect(() => {
    let aborted = false;
    fetch(`/api?q=${query}`)
      .then((r) => r.json())
      .then((d) => {
        if (!aborted) setData(d);
      });
    return () => {
      aborted = true;
    };
  }, [query]); // 用到 query → 放 deps
}
```

**3. React.memo / useMemo / useCallback 何時有效？何時過度優化？**

- React.memo(Component)：避免子元件在 props 淺比較「相等」時重渲染。適用：子元件貴、props 穩定。
- useMemo(fn, deps)：快取計算結果；適用：計算成本高、deps 不常變。
- useCallback(fn, deps)：快取函式身分；適用：把 callback 傳給 memo 子元件或放在依賴等號比較的陣列/Map key。
- 過度優化：計算很便宜、deps 常變（每次都失效）、或增加了比重渲染更大的心智/記憶體負擔。

**4. Suspense for Data Fetching；Error Boundary 的角色**

- Suspense（React 18 起完整）：當子樹「拋出一個 promise」表示尚未就緒時，用 fallback 佔位；配合 Concurrent/Streaming SSR 可做漸進渲染與選擇性 Hydration。
- Error Boundary：攔截 render/生命周期錯誤並渲染後備 UI；不處理事件處理器的錯或非 render 階段的 async 錯（那些要自行 try/catch）。
- 二者不同：Suspense 處理「等待」，Error Boundary 處理「錯誤」；常同時放在資料邊界上：`<ErrorBoundary><Suspense fallback=...>...</Suspense></ErrorBoundary>`。

**5. Controlled vs Uncontrolled；大表單效能策略**

- Controlled：value 由 React state 驅動、每次 onChange 立刻 setState。可預測、易做即時驗證，但多輸入大量更新時可能卡。
- Uncontrolled：值存在 DOM，自行在 submit/ref 時讀取，或用第三方 lib 管理。效能好、重渲染少，但即時管控較麻煩。
- 效能策略：拆分子表單、只讓改動的欄位重渲染（memo + selectors）、debounce 較貴驗證、useDeferredValue/transition 弱化互動延遲、對大表單採 uncontrolled + react-hook-form 類方案（用 refs/代理，少 setState）。

**6. 列表虛擬化取捨；「往上載入不抖動」怎麼做？**

- 虛擬化（react-window / react-virtuoso）：只渲染可視區域，大幅減少 DOM。取捨：滾動量測、項目高度變化、SEO/SSR。
- 往上載入不抖動：以錨點（anchor）補償 scrollTop。載入上一頁（prepend）前記錨點位置，prepend 後計算新位置差值，把 scrollTop += delta 抵消高度變化；或用支援「保持視窗定位」的虛擬化庫內建 API。

**7. 全域狀態：Context vs Redux vs Zustand（或其他）**

- Context：傳遞少量、不常變的值（主題、使用者、i18n）。值一變所有消費者都重渲染；可用 use-context-selector 減少影響。
- Redux：單一 store、不可變更新、select/memo 精準訂閱、開發工具完整、可預測流程。適合中大型應用與複雜協作。
- Zustand：輕量、基於選擇器的細粒度訂閱、學習曲線低。適合中小型或局部狀態。
- 選型關鍵：變更頻率、團隊習慣、開發工具需求；避免用 Context 當頻繁變動的全域 bus。

**8. SSR/Hydration 常見錯誤與排查（Hydration mismatch）**

- 成因：伺服器輸出的 HTML 與客戶端首渲染結果不一致。常見：
  - 使用 非決定性值（時間、亂數、Math.random()、語系不同）
  - 在 render 期間讀取 window/document（伺服器沒有）
  - 依賴 環境差異（時區、locale、A/B）或資料到達時序不同
  - 清單 key 不穩定導致結構不同
- 排查：在本機開 SSR + React.StrictMode，比對伺服器 HTML 與 CSR 首次 render；把動態區塊改到 useEffect 後再設定，或用 suppressHydrationWarning 局部忽略。
  - 解法：
    - 使 render 純函數化、可重放，去除非決定性；
    - 需 瀏覽器 API 的元件用動態載入 ssr: false；
    - 伺服器與客戶端共用同一份資料快照（RSC/defer/Suspense 或資料預取）；
    - 穩定 key 與排序，避免首屏不同步。
