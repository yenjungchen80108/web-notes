---
sidebar_position: 1
---

# Set

集合是由一組無序且唯一的元素組成。

## 集合的特點

1. 無序：集合中的元素沒有順序關係。
2. 唯一：集合中的元素是唯一的，不會有重複的元素。

## 集合可用的方法

1. add(value)：添加一個元素到集合中。
2. delete(value)：從集合中刪除一個元素。
3. has(value)：檢查集合中是否包含一個元素。
4. clear()：清空集合。
5. size：返回集合中元素的數量。
6. values()：返回集合中所有的元素。

## 集合操作

1. 聯集：兩個集合中所有的元素。union
2. 交集：兩個集合中共同的元素。intersection
3. 差集：兩個集合中，第一個集合中，但第二個集合中不包含的元素。difference
4. 對稱差集：兩個集合中，第一個集合中，但第二個集合中不包含的元素，以及第二個集合中，但第一個集合中不包含的元素。symmetricDifference
5. 子集：第一個集合是否是第二個集合的子集。isSubsetOf
6. 超集：第一個集合是否是第二個集合的超集。isSupersetOf

```js
a = new Set();
b = new Set();

a.add(1);
a.add(3);
a.add(4);
a.add(6);
a.add(1);

b.add(2);
b.add(3);
b.add(5);
b.add(7);
b.add(1);

console.log("union", a.union(b)); // {1,3,4,6,2,5,7}
console.log("instersect", a.intersection(b)); // {1,3}
console.log("diff", a.difference(b)); // {4,6}
console.log("symmetricDifference", a.symmetricDifference(b)); // {4,6,2,5,7}
console.log("isSubsetOf", a.isSubsetOf(b)); // false
console.log("isSupersetOf", a.isSupersetOf(b)); // false
```
