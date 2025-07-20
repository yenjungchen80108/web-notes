---
sidebar_position: 5
---

# Quick Sort

Quick Sort 是選擇一個基準點，然後將數列分成兩半，然後將兩半排序，最後將兩半合併。

### 演算法

1. 選擇一個基準點。
2. 將數列分成兩半。
3. 將兩半排序。
4. 將兩半合併。

Quick sort is a **divide-and-conquer** sorting algorithm that works by selecting a **pivot** element and partitioning the array around it.
Elements smaller than the pivot are placed on one side, and elements larger than the pivot are placed on the other.
This process is then recursively applied to the sub-arrays until the entire array is sorted.

Here's a basic implementation in JavaScript:

```js
function quickSort(arr) {
  // Base case: arrays with 0 or 1 element are already sorted
  if (arr.length <= 1) {
    return arr;
  }

  // Choose a pivot (here, the last element is chosen)
  const pivot = arr[arr.length - 1];
  const left = [];
  const right = [];

  // Partition the array
  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] < pivot) {
      left.push(arr[i]);
    } else {
      right.push(arr[i]);
    }
  }

  // Recursively sort the sub-arrays and combine them
  return [...quickSort(left), pivot, ...quickSort(right)];
}

// Example usage:
const unsortedArray = [3, 0, 2, 5, -1, 4, 1];
const sortedArray = quickSort(unsortedArray);
console.log(sortedArray); // Output: [-1, 0, 1, 2, 3, 4, 5]
```

### Explanation:

- Base Case:
  If the input array has zero or one element, it is already sorted, so it is returned directly.
- Pivot Selection:
  A pivot element is chosen. In this example, the last element of the array is selected as the pivot. Other pivot selection strategies exist (e.g., first element, middle element, random element).
- Partitioning:
  The array is iterated through (excluding the pivot), and each element is compared to the pivot. Elements smaller than the pivot are added to the left array, and elements greater than or equal to the pivot are added to the right array.
- Recursion:
  The quickSort function is recursively called on the left and right sub-arrays.
  Combination:
  The sorted left sub-array, the pivot element, and the sorted right sub-array are combined using the spread operator (...) to form the final sorted array.
