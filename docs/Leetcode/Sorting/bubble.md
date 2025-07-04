---
sidebar_position: 1
---

# Bubble Sort

[Visualgo](https://visualgo.net/en/sorting)

Bubble Sort 是一種簡單的排序演算法，它重複地走訪過要排序的數列，一次比較兩個元素，如果他們的順序錯誤就把他們交換過來。走訪數列的工作是重複地進行直到沒有再需要交換，也就是說該數列已經排序完成。這個演算法的名字由來是因為越小的元素會經由交換慢慢「浮」到數列的頂端。

### 演算法

1. 比較相鄰的元素。如果第一個比第二個大，就交換他們兩個。
2. 對每一對相鄰元素作同樣的工作，從開始第一對到最後一對。這步做完後，最後的元素會是最大的數。
3. 針對所有的元素重複以上的步驟，除了最後一個。
4. 持續每次對越來越少的元素重複上面的步驟，直到沒有任何一對數字需要比較。
