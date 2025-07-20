---
sidebar_position: 3
---

# Insertion Sort

Insertion Sort 是重複地將一個元素插入到已排序的數列中。這個演算法的名字由來是因為每次插入一個元素，就像是在插入中插入一個元素。

### 演算法

1. 從第一個元素開始，認為它已經排序。
2. 取出下一個元素，在已排序的數列中從後向前掃描。
3. 如果該元素（已排序）大於新元素，將該元素移到下一位置。

Insertion sort is a simple sorting algorithm that builds a **final sorted array** one item at a time. It is often compared to the way a person sorts a hand of playing cards.

### How it works:

- Divide the array:
  Conceptually, the array is divided into two parts: a **sorted portion** (initially containing just the first element) and an **unsorted portion**.

- Iterate through the unsorted portion:
  The algorithm iterates through the unsorted part of the array, taking one element at a time.
  Insert into the sorted portion:
  For each element taken from the unsorted part, it is compared with elements in the sorted portion, moving from right to left.
- Shift elements:
  If an element in the sorted portion is greater than the current element being inserted, it is shifted one position to the right to make space.
- Place the element:
  This shifting continues until the correct position for the current element is found (where all elements to its left are smaller and all elements to its right are larger). The current element is then inserted into that position.
- Repeat:
  Steps 2-5 are repeated until all elements from the unsorted portion have been inserted into their correct positions within the sorted portion, resulting in a fully sorted array.

Basic JavaScript Implementation:

```js
const insertionSort = (arr) => {
  for (let i = 1; i < arr.length; i++) {
    let currentValue = arr[i]; // Element to be inserted
    let j = i - 1; // Index of the last element in the sorted portion

    // Shift elements in the sorted portion that are greater than currentValue
    while (j >= 0 && arr[j] > currentValue) {
      arr[j + 1] = arr[j];
      j--;
    }

    // Insert currentValue into its correct position
    arr[j + 1] = currentValue;
  }
  return arr;
};

// Example usage:
const unsortedArray = [27, 3, 7, 1, 0];
const sortedArray = insertionSort(unsortedArray);
console.log(sortedArray); // Output: [0, 1, 3, 7, 27]
```
