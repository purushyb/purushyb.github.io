---
title: "Two Pointers: Reversing and De-duplicating Arrays"
description: "The Power of Two Pointers: Reversing and De-duplicating Arrays"
date: 2026-01-02
categories: [Algorithms, Two Pointer Manipulation]
tags: [dsa, kotlin, two-pointers, arrays, interview-prep]
math: true
---

The **Two-Pointer Technique** is a fundamental strategy used to solve array and string problems efficiently. By using two indices that move at different speeds or in different directions, we can often solve problems in $O(n)$ time that would otherwise require $O(n^2)$.

In this post, we’ll explore two classic applications: **Reversing an Array** and **Removing Duplicates from a Sorted Array**.

---

## 1. Reversing an Array (Converging Pointers)

The simplest version of two pointers involves one pointer at the start and one at the end, moving toward each other.

### The Logic
1. Place `leftPointer` at index `0`.
2. Place `rightPointer` at index `n - 1`.
3. Swap the values and move both pointers inward until they meet in the middle.



```kotlin
fun reverseArray(arr: Array<Int>) {
    var leftPointer = 0
    var rightPointer = arr.size - 1

    while (leftPointer < rightPointer) {
        // Swap helper
        val temp = arr[leftPointer]
        arr[leftPointer] = arr[rightPointer]
        arr[rightPointer] = temp
        
        // Move pointers toward center
        leftPointer++
        rightPointer--
    }
}
```
Complexity:
- Time: $O(n)$ — We visit each element exactly once.
- Space: $O(1)$ — We modify the array in-place.

## 2. Removing Duplicates (Slow & Fast Pointers)

This problem requires a slightly different approach. Instead of pointers moving toward each other, we use a Slow Pointer and a Fast Pointer both moving in the same direction.

Pre-requisite: The array must be sorted.

The Logic
- Slow Pointer (leftPointer): Keeps track of the last unique element found.
- Fast Pointer (rightPointer): Scans the array looking for new unique values.

When the fast pointer finds a value that is different from the previous one, we move the slow pointer forward and update its value.

```kotlin
fun removeDuplicates(arr: Array<Int>): Int {
    if (arr.isEmpty()) return 0
    
    var leftPointer = 0

    for (rightPointer in 1..<arr.size) {
        // If we find a new unique element
        if (arr[rightPointer] != arr[rightPointer - 1]) {
            leftPointer++
            arr[leftPointer] = arr[rightPointer]
        }
    }
    
    // Return the new size (index + 1)
    return leftPointer
}
```
Usage
```kotlin
val sortedArray = arrayOf(1, 1, 2, 2, 2, 3)
val newEndIndex = removeDuplicates(sortedArray)

// Use copyOfRange to see the cleaned result
println(sortedArray.copyOfRange(0, newEndIndex + 1).contentToString()) 
// Output: [1, 2, 3]
```

### Why use Two Pointers?

1. Memory Efficiency: Both of these algorithms operate in-place. We don't need to create a second array to hold the results.
2. Speed: By avoiding nested loops, we keep the time complexity linear.
3. Readability: Once you understand the "pointer" mental model, the code becomes much easier to reason about than complex index arithmetic.

### Summary

Two pointers are versatile. Whether you are converging from both ends or using a "lead and follow" approach, this technique is a powerful tool in your DSA toolkit. It is the foundation for more complex algorithms like Sliding Window and Cycle Detection.

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/DSAInplaceManipulation.kt).
{: .prompt-tip }