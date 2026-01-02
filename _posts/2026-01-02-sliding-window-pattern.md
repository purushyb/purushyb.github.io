---
title: "Sliding Window Pattern: Efficient Subarray Operations"
description: "Sliding Window Pattern: Efficient Subarray Operations"
date: 2026-01-02
categories: [Algorithms, Sliding Window]
tags: [dsa, kotlin, sliding-window, arrays, optimization]
---

The **Sliding Window** pattern is a powerful optimization technique used to perform operations on a specific window size of an array or iterable. Instead of recalculating everything from scratch as we move through the data, we "slide" the window by adding the next element and removing the first one.

In this post, we’ll look at two types of sliding windows: **Fixed-size** and **Variable-size (Dynamic)**.

---

## 1. Fixed-Size Window: Maximum Subarray Sum

Imagine you need to find the maximum sum of $k$ consecutive elements in an array. A naive approach would use nested loops ($O(n \times k)$). The sliding window approach does it in $O(n)$.

### The Logic
Instead of re-summing all elements, we subtract the element that is "falling out" of the back of the window and add the new element "entering" the front.



```kotlin
fun maxSubArraySum(arr: Array<Int>, subArraySize: Int): Int {
    var maxSum = 0

    // Initial window sum
    for (i in 0..<subArraySize) {
        maxSum += arr[i]
    }

    var currentSum = maxSum

    // Slide the window
    for (i in subArraySize..<arr.size) {
        // Subtract the leftmost element, add the rightmost
        currentSum = currentSum - arr[i - subArraySize] + arr[i]
        maxSum = maxOf(currentSum, maxSum)
    }

    return maxSum
}
```

## 2. Variable-Size Window: Product Less Than $K$

Sometimes the window doesn't have a fixed size. Instead, the window grows or shrinks based on a specific condition. This is often solved using a Two-Pointer Sliding Window.

### The Logic

We use a rightIndex to expand the window and a leftIndex to shrink it when the condition (Product < $K$) is violated.

```kotlin
fun subArraysWithProductLessThanDesiredProduct(arr: Array<Int>, productDesired: Int): Int {
    if (productDesired <= 1) return 0

    var subArrayCount = 0
    var windowProduct = 1
    var leftIndex = 0

    for (rightIndex in 0..<arr.size) {
        // Expand the window
        windowProduct *= arr[rightIndex]

        // Shrink the window until the condition is met
        while (windowProduct >= productDesired) {
            windowProduct /= arr[leftIndex]
            leftIndex++
        }

        // The number of new subarrays ending at rightIndex is (right - left + 1)
        subArrayCount += (rightIndex - leftIndex) + 1
    }
    return subArrayCount
}
```
### Why does `(rightIndex - leftIndex)` + 1 work?

Every time we add a new element at `rightIndex` that keeps the product within the limit, it creates new valid subarrays. For example, if our window is `[2, 5, 10]` and we add `[3]`, the new valid subarrays are `[3]`, `[10, 3]`, `[5, 10, 3]`, and `[2, 5, 10, 3]`.

## Comparison of Patterns

| Feature | Fixed Window | Variable Window |
| --- | --- | --- |
|**Window Size**|Constant($k$)|Dynamic|
|**Pointers**|One index(usually $i$)|Two pointers(`left`,`right`)|
|**Complexity**|$O(n)$|$O(n)$|
|**Common Use**|Max/Min Sum, Average|Longest Substring, Subarray Count|

## Summary

The Sliding Window pattern is all about reducing redundant work. By reusing the result of the previous window, we achieve linear time complexity. It is one of the most frequently asked patterns in top-tier technical interviews.

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/DSASlidingWindow.kt).
{: .prompt-tip }