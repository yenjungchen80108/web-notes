---
sidebar_position: 4
---

# Merge Sort

Merge Sort 是將數列分成兩半，然後將兩半排序，最後將兩半合併。

### 演算法

1. 將數列分成兩半。
2. 將兩半排序。
3. 將兩半合併。

Merge sort is a sorting algorithm that follows the **divide and conquer** paradigm. It works by recursively breaking down an array into smaller subarrays until each subarray contains only one element (which is inherently sorted). Then, these single-element subarrays are merged back together in a sorted manner.

Here's a basic implementation of Merge Sort in JavaScript:

```js
function mergeSort(arr) {
  // Base case: if the array has 0 or 1 element, it's already sorted
  if (arr.length <= 1) {
    return arr;
  }

  // Divide the array into two halves
  const middle = Math.floor(arr.length / 2);
  const left = arr.slice(0, middle);
  const right = arr.slice(middle);

  // Recursively sort the two halves
  const sortedLeft = mergeSort(left);
  const sortedRight = mergeSort(right);

  // Merge the sorted halves
  return merge(sortedLeft, sortedRight);
}

function merge(left, right) {
  let result = [];
  let leftIndex = 0;
  let rightIndex = 0;

  // Compare elements from left and right arrays and add the smaller one to the result
  while (leftIndex < left.length && rightIndex < right.length) {
    if (left[leftIndex] < right[rightIndex]) {
      result.push(left[leftIndex]);
      leftIndex++;
    } else {
      result.push(right[rightIndex]);
      rightIndex++;
    }
  }

  // Add any remaining elements from the left array
  while (leftIndex < left.length) {
    result.push(left[leftIndex]);
    leftIndex++;
  }

  // Add any remaining elements from the right array
  while (rightIndex < right.length) {
    result.push(right[rightIndex]);
    rightIndex++;
  }

  return result;
}

// Example usage:
const unsortedArray = [38, 27, 43, 10, 82, 9, 1];
const sortedArray = mergeSort(unsortedArray);
console.log(sortedArray); // Output: [1, 9, 10, 27, 38, 43, 82]
```

### Explanation:

- mergeSort(arr) function:
  - Base Case: If the input arr has 0 or 1 element, it's already sorted, so it's returned directly.
  - Divide: The array is split into two halves: left and right.
  - Conquer (Recursion): mergeSort is called recursively on both left and right halves to sort them.
  - Combine: The merge function is called to combine the two sorted halves into a single, sorted array.
- merge(left, right) function:
  - This function takes two already sorted arrays, left and right, as input.
  - It initializes an empty result array.
  - It uses two pointers, leftIndex and rightIndex, to iterate through the left and right arrays, respectively.
  - It compares the elements at left[leftIndex] and right[rightIndex] and pushes the smaller element into the result array, incrementing the corresponding index.
  - After one of the arrays is exhausted, it adds any remaining elements from the other array to the result.
  - Finally, it returns the merged, sorted result array.
