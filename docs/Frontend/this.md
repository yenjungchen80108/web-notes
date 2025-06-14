---
sidebar_position: 8
---

# `this` Keyword in JavaScript

this 是 JavaScript 中的關鍵字，用於指向當前執行上下文。
在不同情況下，this 的值會有所不同。

```javascript
const fullname = "apple";
const obj = {
  fullname: "banana",
  lastname: "orange",
  prop: {
    getFullname: () => {
      return this.fullname;
    },
    getLastname: function () {
      return this.lastname;
    },
  },
};

console.log(obj.prop.getFullname());
console.log(obj.prop.getLastname());
const test = obj.prop.getFullname;
console.log(test());
```

## 解釋

1. obj.prop.getFullname() 是箭頭函數，箭頭函數的 this 不會根據呼叫者綁定，而是繼承自定義他的地方的 this，所以 this 會指向 obj.prop 的 this，也就是 obj.prop 的 this 是 window

> 瀏覽器中：
> 這邊定義的是 const fullname = "apple"， 不是 window.fullname
> 如定義 window.fullname = "apple"， 則會輸出 apple

> node 中：
> this.fullname 指的是 module 頂層 this，通常是 {}，所以是 undefined

2. obj.prop.getLastname() 是普通函數，this 根據呼叫方式決定。呼叫方式是 obj.prop.getLastname()，所以 this 會指向 obj.prop。但是 obj.prop 中沒有 lastname，所以是 undefined

> 假設定義 obj.prop.lastname = "orange"，則會輸出 orange

3. const test = obj.prop.getFullname; test(); 這是把箭頭函數賦值後調用，箭頭函數的 this 不會根據呼叫者綁定，而是繼承自定義他的地方的 this，所以 this 會指向 window

> 結果也是 undefined

## 總結

1. 箭頭函數的 this 不會根據呼叫者綁定，而是繼承自定義他的地方的 this
2. 普通函數的 this 根據呼叫方式決定
