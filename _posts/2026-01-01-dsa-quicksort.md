---
title: "QuickSort: The Divide and Conquer King"
description: "Mastering QuickSort in Kotlin: The Divide and Conquer King"
date: 2026-01-01
categories: [Algorithms, quick sort]
tags: [dsa, sorting, kotlin, quicksort, algorithms]
math: true
---

If Binary Search is the king of searching, **QuickSort** is often considered the king of sorting. It is the algorithm that powers many standard library sort functions because of its efficiency and cache performance.

In this post, we'll implement QuickSort in Kotlin using the **Lomuto Partition Scheme**. We'll cover sorting simple integers and then move to a real-world scenario: sorting a list of database records by ID.

## The Concept

QuickSort is a **Divide and Conquer** algorithm. Unlike Merge Sort, which divides the array in half blindly, QuickSort divides the array based on values.

The core logic revolves around a **Pivot**.
1.  **Pick a Pivot:** We select an element (in our case, the last element).
2.  **Partition:** We rearrange the array so that all elements **smaller** than the pivot move to the left, and all elements **larger** move to the right.
3.  **Recursion:** We recursively apply the same logic to the left and right sub-arrays.



## 1. The Standard Implementation (Integers)

We use the **Lomuto partition scheme**, which is generally easier to understand and implement than the original Hoare partition.

* **Pivot:** Always the last element (`arr[high]`).
* **i (indexToSwap):** Tracks the boundary of the "smaller elements" region.

```kotlin
fun main() {
    val arr = arrayOf<Int>(8, 4, 7, 9, 10, 5)
    dsaQuickSort(arr, 0, arr.size - 1)
    println(arr.contentToString()) // Output: [4, 5, 7, 8, 9, 10]
}

fun dsaQuickSort(arr: Array<Int>, low: Int, high: Int) {
    if (low < high) {
        // Get the partition index
        val pivotIndex = partition(arr, low, high)
        
        // Recursively sort elements before and after partition
        dsaQuickSort(arr, low, pivotIndex - 1)
        dsaQuickSort(arr, pivotIndex + 1, high)
    }
}

// Lomuto partition scheme
fun partition(arr: Array<Int>, low: Int, high: Int): Int {
    val pivot = arr[high]
    var indexToSwap = low - 1 // Index of smaller element

    for (j in low..<high) {
        // If current element is smaller than the pivot
        if (arr[j] < pivot) {
            indexToSwap++
            swap(arr, indexToSwap, j)
        }
    }
    // Place the pivot in the correct position
    swap(arr, indexToSwap + 1, high)
    return indexToSwap + 1
}

fun swap(arr: Array<Int>, i: Int, j: Int) {
    val temp = arr[i]
    arr[i] = arr[j]
    arr[j] = temp
}
```
## 2. Real-World Scenario: Sorting Objects
In production, you are likely sorting objects, such as a list of users or database records, based on a specific field (like id or timestamp).

Here is how we adapt the algorithm for a DataBaseRecord class.

```kotlin
data class DataBaseRecord(val id: Int, val name: String)

fun main() {
    val nodesToSort = arrayOf(
        DataBaseRecord(id = 8, name = "Anjana"),
        DataBaseRecord(id = 4, name = "Badri"),
        DataBaseRecord(id = 7, name = "Catherine"),
        DataBaseRecord(id = 9, name = "Donnie"),
        DataBaseRecord(id = 10, name = "Elene"),
        DataBaseRecord(id = 5, name = "Fatima")
    )

    dsaQuickSortGeneric(nodesToSort, 0, nodesToSort.size - 1)
    
    // Output will show records sorted by ID: 4, 5, 7, 8, 9, 10
    println(nodesToSort.contentToString())
}

fun dsaQuickSortGeneric(arr: Array<DataBaseRecord>, low: Int, high: Int) {
    if (low < high) {
        val pivotIndex = partition(arr, low, high)
        dsaQuickSortGeneric(arr, low, pivotIndex - 1)
        dsaQuickSortGeneric(arr, pivotIndex + 1, high)
    }
}

fun partition(arr: Array<DataBaseRecord>, low: Int, high: Int): Int {
    val pivot = arr[high]
    var indexToSwap = low - 1

    for (j in low..<high) {
        // Compare based on the 'id' property
        if (arr[j].id < pivot.id) {
            indexToSwap++
            swap(arr, j, indexToSwap)
        }
    }

    swap(arr, indexToSwap + 1, high)
    return indexToSwap + 1
}

fun swap(arr: Array<DataBaseRecord>, i: Int, j: Int) {
    val temp = arr[i]
    arr[i] = arr[j]
    arr[j] = temp
}
```

### Complexity Analysis

Why do we use QuickSort?
- Time Complexity (Average): $O(n \log n)$. This is the standard for efficient sorting.
- Time Complexity (Worst Case): $O(n^2)$. This happens when the array is already sorted (or reverse sorted) and we pick the last element as the pivot. In this case, our partition is very unbalanced (0 elements on one side, n-1 on the other).
- Space Complexity: $O(\log n)$ due to the recursive stack frames.

### Why not Merge Sort?
Merge Sort guarantees $O(n \log n)$ even in the worst case, but it requires $O(n)$ extra space to create new arrays during the merge step. QuickSort sorts in-place, making it more memory efficient and often faster in practice due to better cache locality.

## Summary
QuickSort is a must-know algorithm for technical interviews. The key to mastering it is understanding the partition function. Once you understand how the pivot moves elements around it, the recursive structure becomes easy to visualize.

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/DSAQuickSort.kt).
{: .prompt-tip }