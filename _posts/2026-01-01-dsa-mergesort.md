---
title: "Merge Sort: The Stable Divide and Conquer Algorithm"
description: "Mastering Merge Sort in Kotlin: The Stable Divide and Conquer Algorithm"
date: 2026-01-01
categories: [Algorithms, Merge Sort]
tags: [dsa, sorting, kotlin, mergesort, algorithms]
math: true
---

While QuickSort is fast, **Merge Sort** is reliable. It guarantees $O(n \log n)$ time complexity even in the worst-case scenario and is a **stable** sort—meaning it preserves the original order of elements with equal keys.

In this post, we will implement Merge Sort in Kotlin to sort integers and then use idiomatic Kotlin features to sort custom Database objects.

## The Concept

Merge Sort follows the **Divide and Conquer** strategy:
1.  **Divide:** Recursively split the array in half until you have sub-arrays of size 1.
2.  **Conquer:** Merge those sub-arrays back together in sorted order.



Unlike QuickSort, which does the heavy lifting during the "partition" phase, Merge Sort does the work during the "merge" phase after the recursive calls return.

## 1. Sorting Integers

The logic is split into two parts: the recursive split (`dsaMergeSort`) and the sorting logic (`merge`).

```kotlin
fun dsaMergeSort(arr: Array<Int>, leftIndex: Int, rightIndex: Int) {
    if (leftIndex < rightIndex) {
        val mid = leftIndex + (rightIndex - leftIndex) / 2

        // Recursively split
        dsaMergeSort(arr, leftIndex, mid)
        dsaMergeSort(arr, mid + 1, rightIndex)

        // Merge the sorted halves
        merge(arr, leftIndex, mid, rightIndex)
    }
}

fun merge(arr: Array<Int>, low: Int, mid: Int, high: Int) {
    val leftArrLength = mid - low + 1
    val rightArrLength = high - mid

    // Create temporary arrays and copy data
    val leftArr = Array(leftArrLength) { 0 }
    val rightArr = Array(rightArrLength) { 0 }

    for (i in 0..<leftArrLength) leftArr[i] = arr[low + i]
    for (j in 0..<rightArrLength) rightArr[j] = arr[mid + 1 + j]

    var leftArrIndex = 0
    var rightArrIndex = 0
    var arrIndex = low

    // Compare and place elements back into original array
    while (leftArrIndex < leftArrLength && rightArrIndex < rightArrLength) {
        if (leftArr[leftArrIndex] <= rightArr[rightArrIndex]) {
            arr[arrIndex] = leftArr[leftArrIndex++]
        } else {
            arr[arrIndex] = rightArr[rightArrIndex++]
        }
        arrIndex++
    }

    // Clean up remaining elements
    while (leftArrIndex < leftArrLength) arr[arrIndex++] = leftArr[leftArrIndex++]
    while (rightArrIndex < rightArrLength) arr[arrIndex++] = rightArr[rightArrIndex++]
}
```

## 2. Optimized Merging with `copyOfRange`
In Kotlin, we can simplify the merge step significantly using copyOfRange. This function is highly optimized and returns a new array containing the specified range.

Note: copyOfRange(from, to) is exclusive of the to index, which is why we use mid + 1 and high + 1.

```kotlin
fun merge(arr: Array<Int>, low: Int, mid: Int, high: Int) {
    // Optimized copying using Kotlin's Standard Library
    val leftArr = arr.copyOfRange(low, mid + 1)
    val rightArr = arr.copyOfRange(mid + 1, high + 1)

    var leftIndex = 0
    var rightIndex = 0
    var arrIndex = low

    // Comparison logic
    while (leftIndex < leftArr.size && rightIndex < rightArr.size) {
        if (leftArr[leftIndex] <= rightArr[rightIndex]) {
            arr[arrIndex] = leftArr[leftIndex++]
        } else {
            arr[arrIndex] = rightArr[rightIndex++]
        }
        arrIndex++
    }

    // Transfer remaining elements
    while (leftIndex < leftArr.size) arr[arrIndex++] = leftArr[leftIndex++]
    while (rightIndex < rightArr.size) arr[arrIndex++] = rightArr[rightIndex++]
}
```

## 3. Sorting Objects: DataBaseRecord

The same logic applies to custom objects. By using copyOfRange, we avoid manual loops and the "dummy object" initialization issue, keeping our code clean and performant.

```kotlin
fun mergeDataBaseRecords(arr: Array<DataBaseRecord>, low: Int, mid: Int, high: Int) {
    // copyOfRange returns a new Array of the same type
    val leftArr = arr.copyOfRange(low, mid + 1)
    val rightArr = arr.copyOfRange(mid + 1, high + 1)

    var i = 0
    var j = 0
    var k = low

    while (i < leftArr.size && j < rightArr.size) {
        if (leftArr[i].id < rightArr[j].id) {
            arr[k] = leftArr[i++]
        } else {
            arr[k] = rightArr[j++]
        }
        k++
    }

    while (i < leftArr.size) arr[k++] = leftArr[i++]
    while (j < rightArr.size) arr[k++] = rightArr[j++]
}
```

### Complexity Analysis

- Time Complexity: $O(n \log n)$ (Best, Average, and Worst case).
- Space Complexity: $O(n)$. This is the main drawback of Merge Sort. We need to allocate temporary arrays (leftArr and rightArr) proportional to the input size during the merge process.

## Summary

Merge Sort is the go-to algorithm when stability is required or when the data is too large to fit into memory (External Sorting), as its predictable memory access patterns are excellent for handling linked lists or disk-based storage.

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/DSAMergeSort.kt).
{: .prompt-tip }