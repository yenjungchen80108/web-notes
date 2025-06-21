---
sidebar_position: 12
---

# Call, Apply, Bind

## 用法

⸻

🧠 概念快速總覽

| 方法  | 功能                                       | 差異點                               |
| ----- | ------------------------------------------ | ------------------------------------ |
| call  | 呼叫函數，同時指定 this 並逐個傳參數       | fn.call(thisArg, arg1, arg2)         |
| apply | 呼叫函數，指定 this 並用`陣列`傳參數       | fn.apply(thisArg, [arg1, arg2])      |
| bind  | 回傳一個`新的函數`，綁定了 this 和預設參數 | const newFn = fn.bind(thisArg, arg1) |

⸻

✅ 1. call 範例：直接呼叫、傳入 this 與參數

```js
function greet(greeting, punctuation) {
  console.log(`${greeting}, my name is ${this.name}${punctuation}`);
}

const person = { name: "Emily" };

greet.call(person, "Hello", "!"); // Hello, my name is Emily!
```

📝 練習

```js
function showAge(year) {
  console.log(`${this.name} is ${2025 - year} years old.`);
}

const you = { name: "Alice" };
showAge.call(you, 1995); // Alice is 30 years old.
```

⸻

✅ 2. apply 範例：與 call 類似，但參數用陣列傳

```js
function introduce(city, country) {
  console.log(`I'm ${this.name} from ${city}, ${country}.`);
}

const user = { name: "Daniel" };
introduce.apply(user, ["Taipei", "Taiwan"]); // I'm Daniel from Taipei, Taiwan.
```

📝 練習

```js
function sum(a, b, c) {
  console.log(`${this.label}: ${a + b + c}`);
}

const data = { label: "Total" };
sum.apply(data, [1, 2, 3]); // Total: 6
```

⸻

✅ 3. bind 範例：不立即執行，回傳一個「綁定後的函式」

```js
function sayHi() {
  console.log(`Hi, I'm ${this.name}`);
}

const dev = { name: "Mia" };
const boundSayHi = sayHi.bind(dev);

boundSayHi(); // Hi, I'm Mia
```

📝 進階版綁參數

```js
function multiply(x, y) {
  return x * y;
}

const double = multiply.bind(null, 2); // 第一個參數是 this，這裡不用所以傳 null
console.log(double(5)); // 10
```

⸻

🧪 小挑戰：比較 call, apply, bind

```js
function fullName(city) {
  return `${this.first} ${this.last} from ${city}`;
}

const student = { first: "Jane", last: "Doe" };

console.log(fullName.call(student, "Sydney")); // call → Sydney
console.log(fullName.apply(student, ["Melbourne"])); // apply → Melbourne
const boundFullName = fullName.bind(student);
console.log(boundFullName("Brisbane")); // bind → Brisbane
```

⸻

🔚 總結重點

| 用法  | 立即執行       | 傳參方式   | 常見用途                   |
| ----- | -------------- | ---------- | -------------------------- |
| call  | ✅             | 一個個參數 | 快速觸發函數，改變 this    |
| apply | ✅             | 陣列參數   | 不定參數或與 Math.max 搭配 |
| bind  | ❌（回傳函數） | 一個個參數 | 延遲執行，提前綁定 this    |

## 為什麼現在很少使用 call、apply、bind？

⸻

✅ 1. Functional Component 取代 Class Component

以前的 React Class Component 中，this 是個大問題：

```js
class MyComponent extends React.Component {
  constructor() {
    super();
    this.handleClick = this.handleClick.bind(this); // 👈 不綁定會出錯
  }

  handleClick() {
    console.log(this); // 預期是 this.component，但不綁就不是
  }
}
```

因為 handleClick 被作為回調傳下去時，this 會丟失。

🔁 所以以前 React 頻繁使用 bind(this)，甚至常常在 constructor 裡一堆 bind。

現在 React 幾乎全用 Function Component + Hooks：

```js
const MyComponent = () => {
  const handleClick = () => {
    console.log("no more this problem");
  };
};
```

💡 箭頭函數會自動 capture 外層作用域，不用手動綁定 this！

⸻

✅ 2. Modern JavaScript 趨勢是更少 this

ES6+ 開始的趨勢就是：能不用 this 就不用 this。
• Arrow functions 📌 沒有自己的 this
• Module scopes 📌 不需要用 this 存共用變數
• React Hooks 📌 全都用 closure 處理狀態
• Class 變得冷門，只用在特殊場合（e.g. lib 架構）

也就是說，語言跟框架都在避開 this 問題，當然也就不再需要 call/apply/bind。

⸻

✅ 3. call / apply / bind 常見於：

這些用途仍然會用得到：

場景 為什麼還會用到
低階函式庫 像 lodash 的 throttle、debounce 內部常綁 this
class-based OO code 比如自訂 event handler 類別時
需要動態切換 context 的場景 如使用 Function.prototype.call 來借用其他物件的方法
手動觸發函式並帶參數與 this 類似 fn.call(this, a, b) 或 fn.apply(this, [a, b])

⸻

✅ 現代替代品：箭頭函數 + 關閉式設計

```js
function createCounter() {
  let count = 0;
  return () => {
    count++;
    console.log(count);
  };
}
```

這樣的函式根本不需要 this，因為狀態透過閉包保存，也不需要 bind。

⸻

🎯 結論

在 React Function Component 為主的現代寫法中，call / apply / bind 幾乎不再必要，因為根本沒在用 this。

這三個方法仍然是 JavaScript 的底層機制，學起來有價值，但你不會天天用它，除非你在寫：
• 老的 class-based 架構
• 原生 DOM 綁定處理器
• 某些 advanced library 開發
