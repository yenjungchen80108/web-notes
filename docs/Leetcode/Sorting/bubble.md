---
sidebar_position: 1
---

# Bubble Sort

[Visualgo](https://visualgo.net/en/sorting)

Bubble Sort 是重複地走訪過要排序的數列，一次比較兩個元素，如果他們的順序錯誤就把他們交換過來。走訪數列的工作是重複地進行直到沒有再需要交換，也就是說該數列已經排序完成。這個演算法的名字由來是因為越小的元素會經由交換慢慢「浮」到數列的頂端。

### 演算法

1. 比較相鄰的元素。如果第一個比第二個大，就交換他們兩個。
2. 對每一對相鄰元素作同樣的工作，從開始第一對到最後一對。這步做完後，最後的元素會是最大的數。
3. 針對所有的元素重複以上的步驟，除了最後一個。
4. 持續每次對越來越少的元素重複上面的步驟，直到沒有任何一對數字需要比較。

Bubble sort is a simple sorting algorithm that repeatedly steps through the list, compares adjacent elements, and **swaps** them if they are in the wrong order.
The largest (or smallest) element "bubbles" to its correct position with each pass.
Here's a basic implementation of bubble sort in JavaScript:

```js
function bubbleSort(arr) {
  let n = arr.length;
  // Outer loop for passes through the array
  for (let i = 0; i < n - 1; i++) {
    // Inner loop for comparing adjacent elements in each pass
    for (let j = 0; j < n - 1 - i; j++) {
      // Compare adjacent elements
      if (arr[j] > arr[j + 1]) {
        // Swap if they are in the wrong order
        let temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
  }
  return arr;
}

// Example usage:
const unsortedArray = [64, 34, 25, 12, 22, 11, 90];
const sortedArray = bubbleSort(unsortedArray);
console.log(sortedArray); // Output: [11, 12, 22, 25, 34, 64, 90]
```

### Explanation:

- Outer Loop (i loop):
  This loop controls the number of passes through the array. In each pass, at least one element "bubbles" to its correct sorted position at the end of the unsorted portion of the array. The `n - 1` ensures we don't go out of bounds.
- Inner Loop (j loop):
  This loop iterates through the unsorted portion of the array, comparing adjacent elements. The `n - 1 - i` in the condition is an optimization: as the outer loop progresses, the largest elements are already sorted at the end, so we don't need to compare them again.
- Comparison and Swap:
  Inside the inner loop, if `(arr[j] > arr[j + 1])` checks if the current element is greater than the next element. If it is, they are swapped using a temporary variable temp to ensure the correct order.
  This implementation sorts the array in ascending order. To sort in descending order, the comparison `arr[j] > arr[j + 1]` would be changed to `arr[j] < arr[j + 1]`.
