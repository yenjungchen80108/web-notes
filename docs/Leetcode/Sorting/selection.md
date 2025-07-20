---
sidebar_position: 2
---

# Selection Sort

Selection Sort 是重複地找到最小元素，並將它移到數列的開頭。這個演算法的名字由來是因為每次選擇最小的元素，就像是在選擇中選出最小的元素。

### 演算法

1. 找到數列中最小的元素。
2. 將它與數列的第一個元素交換。
3. 重複上述步驟，直到所有元素都排序完成。

Selection sort is a simple, in-place comparison-based sorting algorithm. It works by repeatedly finding the **minimum element** from the unsorted part of the array and placing it at the **beginning** of the sorted part.
Here's a basic implementation of selection sort in JavaScript:

```js
function selectionSort(arr) {
  const n = arr.length;

  // Outer loop iterates through the array to build the sorted part
  for (let i = 0; i < n - 1; i++) {
    // Assume the current element is the minimum
    let minIndex = i;

    // Inner loop finds the actual minimum in the unsorted part
    for (let j = i + 1; j < n; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j; // Update minIndex if a smaller element is found
      }
    }

    // If the minimum element is not at the current position 'i', swap them
    if (minIndex !== i) {
      // ES6 array destructuring for swapping elements
      [arr[i], arr[minIndex]] = [arr[minIndex], arr[i]];
    }
  }
  return arr;
}

// Example usage:
const myArray = [64, 25, 12, 22, 11];
console.log("Original array:", myArray);
const sortedArray = selectionSort(myArray);
console.log("Sorted array:", sortedArray); // Output: [11, 12, 22, 25, 64]
```

### How it works:

- Initialization:
  The array is conceptually divided into two parts: a sorted subarray (initially empty) and an unsorted subarray (the entire input array).
- Outer Loop:
  The outer for loop iterates from the first element up to the second-to-last element `(n - 1)`. In each iteration i, it aims to find the correct position for the i-th smallest element.
- Finding the Minimum:
  The inner for loop (starting from `i + 1`) traverses the unsorted portion of the array to find the index (minIndex) of the smallest element.
- Swapping:
  If the smallest element found (arr[minIndex]) is not already at the current position i, the elements at `arr[i]` and `arr[minIndex]` are swapped. This moves the smallest element to its correct sorted position.
- Repetition:
  This process repeats for each subsequent element, effectively growing the sorted subarray from the left until the entire array is sorted.
