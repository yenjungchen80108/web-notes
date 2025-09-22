一、JavaScript（核心語言）

1. setTimeout(...,0), Promise.resolve().then(...), queueMicrotask(...) 的執行順序？為什麼？

2. this 在箭頭函式與一般函式的差異？call/apply/bind 各自什麼時候用？

3. 解釋 hoisting 與 TDZ（Temporal Dead Zone）；var/let/const 的差別。

4. 什麼是 closure？講一個 closure 造成 memory leak 的例子與修法。

5. 深拷貝 vs 淺拷貝：structuredClone、JSON.stringify、遇到 Date/Map/Function 的限制？

6. 手寫 debounce 和 throttle，各自適合哪些 UX 場景？

7. Immutable 更新為什麼有助於效能？講講「物件同一性」與 diff。

8. 請說明事件代理（event delegation）與為何能減少記憶體／提升效能。

⸻

**1. setTimeout vs Promise.then vs queueMicrotask**

- 同一個 宏任務 結束後，瀏覽器會清空所有微任務隊列，然後才進入渲染/下一個宏任務。
- 微任務：Promise.then/catch/finally、queueMicrotask、MutationObserver。
- 宏任務：setTimeout/setInterval、I/O、message 事件等。
- 故：Promise.then / queueMicrotask 先，setTimeout 後。

```js
console.log(1);
setTimeout(() => console.log(2));
Promise.resolve().then(() => console.log(3));
queueMicrotask(() => console.log(4));
console.log(5);
// 輸出：1 5 3 4 2
```

**2. this、箭頭函式 vs 一般函式；call/apply/bind**

- 一般函式的 this 在呼叫點決定：obj.fn() → this===obj；fn() → this===undefined（strict）。
- call/apply/bind 可顯式指定 this；new 會忽略 bind 的 this。
- 箭頭函式：this 來自外層詞法作用域；不能作為 new 的建構子；也沒有 arguments。

```js
const obj = {
  x: 1,
  m() {
    setTimeout(function () {
      console.log(this.x);
    }, 0);
  }, // undefined
  n() {
    setTimeout(() => {
      console.log(this.x);
    }, 0);
  }, // 1 (捕捉外層 this)
};
obj.m();
obj.n();
```

**3. Hoisting 與 TDZ**

let/const 也會被「提升（hoist）」到作用域頂端，但在初始化前落在 TDZ（暫時性死區），存取會拋錯。

- var：宣告提升且初始化為 undefined。
- function 宣告：整個函式可先用（被完整提升）。
- let/const：提升但不初始化，在 TDZ 內存取會 ReferenceError；初始化後才能用。

```js
console.log(a); // undefined  (var hoist)
var a = 1;

console.log(b); // ReferenceError (TDZ)
let b = 2;

foo(); // ok
function foo() {}
```

**4. Closure 與造成記憶體外洩的例子**

閉包：函式攜帶著對其外層詞法環境的引用，即使外層已返回。

- 洩漏案例：長壽命的閉包（計時器／事件監聽）抓著大物件或 DOM 不放。

小例（洩漏 & 修法）

```js
function attach() {
  const big = new Array(1e6).fill("x"); // 大物件
  const handler = () => console.log(big.length); // 閉包抓住 big

  window.addEventListener("scroll", handler);
  // 洩漏：沒移除就一直活著 → big 無法被 GC
}
// 修：在適當時機 removeEventListener，或把 big 改為可`釋放的弱引用
```

**5. Shallow vs Deep copy**

淺拷貝只拷「第一層」值，巢狀引用仍共享；深拷貝則遞迴複製。

- 淺拷貝：`{...obj}, Object.assign({}, obj)`。巢狀物件共享引用。
- 深拷貝：`structuredClone(obj)`（現代瀏覽器/Node）、`lodash.cloneDeep`。
- `JSON.parse(JSON.stringify(obj))`：會丟 Date、Map/Set、undefined、Infinity、函式，且不能處理循環引用。

```js
const a = { n: { x: 1 } };
const b = { ...a }; // shallow
b.n.x = 2; // a.n.x 也變 2

const c = structuredClone(a); // deep
c.n.x = 3; // a.n.x 不變
```

**6. debounce vs throttle**

- debounce：可省略 trailing。
- throttle：先執行 leading，最後 trailing（首/尾）。

```js
function debounce(fn, wait, { leading = false, trailing = true } = {}) {
  let t,
    invoked = false;
  return function (...args) {
    if (leading && !t && !invoked) {
      fn.apply(this, args);
      invoked = true;
    }
    clearTimeout(t);
    t = setTimeout(() => {
      if (trailing) fn.apply(this, args);
      t = null;
      invoked = false;
    }, wait);
  };
}

function throttle(fn, wait) {
  let last = 0,
    timer = null,
    savedArgs,
    savedThis;
  return function (...args) {
    const now = Date.now();
    const run = () => {
      last = now;
      timer = null;
      fn.apply(savedThis, savedArgs);
    };
    savedArgs = args;
    savedThis = this;
    if (now - last >= wait) run();
    else if (!timer) timer = setTimeout(run, wait - (now - last));
  };
}
```

**7. 為何「不可變更新」有助效能**

- React/Redux 透過 淺比較（shallow compare） 判斷是否重渲染。
- 不可變更新（回傳新物件/新陣列）讓比較是 O(1)（比引用），而不是深度走訪。
- 好處：記憶化（memo）、shouldComponentUpdate / React.memo、時間旅行（state history）。

**8. 事件代理（Event Delegation）**

- 原理：事件冒泡；把多個子元素的監聽移到父層 container.addEventListener('click', ...)，依 event.target 判斷來源。
- 好處：更少監聽器、動態內容自動生效。
- 注意：某些事件不冒泡（如 blur/focus，可用 focusin/focusout），以及 stopPropagation 會中斷冒泡；使用 event.currentTarget vs event.target 区分誰被綁、誰觸發。
