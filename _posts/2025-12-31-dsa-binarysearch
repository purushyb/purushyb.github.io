---
title: "Mastering Binary Search in Kotlin: From Basics to Visualization"
date: 2025-12-31 12:00:00 +1100
categories: [Algorithms, Kotlin]
tags: [dsa, binary search, algorithms, tutorial]
math: true
---

Binary Search is one of the first efficient algorithms developers learn, but implementing it cleanly—and understanding exactly how it moves—is a vital skill. 

In this post, we'll explore Binary Search in Kotlin. We will look at a standard integer implementation, a "real-world" object search (simulating a database record lookup), and finally, a custom visualizer to see the algorithm in action.

## The Concept

Binary Search works on a **divide and conquer** strategy. The prerequisite is that the collection **must be sorted**. Instead of checking every element (Linear Search, $O(n)$), we check the middle element.

1. If the middle is the target: We are done.
2. If the target is larger than the middle: We discard the left half.
3. If the target is smaller than the middle: We discard the right half.

This reduces the search space by half every iteration, resulting in a time complexity of $O(\log n)$.



## 1. The Standard Implementation

Let's look at the classic iterative approach in Kotlin. We use two pointers, `leftIndex` and `rightIndex`, to define our search window.

```kotlin
fun dsaBinarySearch(arrayToSearch: Array<Int>, elementToFind: Int): Int {
    var leftIndex = 0
    var rightIndex = arrayToSearch.size - 1
    var midIndex: Int

    while (leftIndex <= rightIndex) {
        // Calculate middle preventing potential overflow in massive arrays
        // though (L+R)/2 is fine for standard usage.
        midIndex = (rightIndex + leftIndex) / 2

        if (arrayToSearch[midIndex] == elementToFind) {
            return midIndex
        } else if (elementToFind > arrayToSearch[midIndex]) {
            // Target is in the right half
            leftIndex = midIndex + 1
        } else {
            // Target is in the left half
            rightIndex = midIndex - 1
        }
    }

    return -1 // Element not found
}
```

## 2. Searching Objects (The "Real World" Scenario)

In actual app development, we rarely search simple arrays of integers. We usually search lists of Objects (like a User or Product) based on an ID.

Here is a DataBaseRecord data class and a modified search function that looks up records by their id.

```kotlin
data class DataBaseRecord(val id: Int, val name: String)

fun dsaBinarySearchGeneric(elementsToSearch: Array<DataBaseRecord>, idToSearch: Int): Int {
    var leftIndex = 0
    var rightIndex = elementsToSearch.size - 1
    var midIndex: Int

    while (leftIndex <= rightIndex) {
        midIndex = (rightIndex + leftIndex) / 2
        
        // Compare the target ID with the Object's ID
        if (idToSearch == elementsToSearch[midIndex].id) {
            return midIndex
        } else if (idToSearch > elementsToSearch[midIndex].id) {
            leftIndex = midIndex + 1
        } else {
            rightIndex = midIndex - 1
        }
    }
    return -1
}
```

### Usage Example
```kotlin
val nodesToSearch = arrayOf(
    DataBaseRecord(id = 1, name = "Anjana"),
    DataBaseRecord(id = 3, name = "Badri"),
    DataBaseRecord(id = 17, name = "Ishika")
    // ... assumed sorted by ID
)

// Returns the index of "Ishika"
dsaBinarySearchGeneric(nodesToSearch, 17)
```

## 3. Visualizing the Algorithm
Debugging algorithms can be tough. Sometimes, printing the state of your pointers (L, R, and M) helps you understand exactly how the window shrinks.
Here is a custom function that draws the search window to the console.
```kotlin
fun dsaBinarySearchVisual(elements: Array<Int>, target: Int): Int {
    var left = 0
    var right = elements.size - 1
    var mid: Int

    println("Target: $target")
    println("Index:  " + elements.indices.joinToString("  ") { "%2d".format(it) })
    println("Values: " + elements.joinToString("  ") { "%2d".format(it) })
    println("-".repeat(elements.size * 4 + 8))

    while (left <= right) {
        mid = (right + left) / 2

        // --- Visualization Logic ---
        val visualLine = StringBuilder("        ")
        for (i in elements.indices) {
            val marker = when (i) {
                mid -> "M " // Midpoint
                left -> "L " // Left bound
                right -> "R " // Right bound
                else -> "  "
            }
            visualLine.append(" $marker ")
        }
        println(visualLine.toString() + "  (Range: $left-$right, Mid: $mid)")
        // ---------------------------

        if (target == elements[mid]) {
            println("Found at index $mid!")
            return mid
        } else if (target > elements[mid]) {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return -1
}
```

### The Output
If we search for the number 17 in a list of 10 numbers, here is what the console output looks like. Watch how L (Left) moves past M to shrink the window.

```
Target: 17
Index:   0   1   2   3   4   5   6   7   8   9
Values:  1   3   5   7   9  11  13  15  17  19
------------------------------------------------
         L               M                   R    (Range: 0-9, Mid: 4)
                             L       M       R    (Range: 5-9, Mid: 7)
                                         LM  R    (Range: 8-9, Mid: 8)
Found at index 8!
```
## Summary
- Pre-requisite: Data must be sorted.
- Time Complexity: $O(\log n)$.
- Space Complexity: $O(1)$ (Iterative).

Binary search is the foundation for many complex algorithms. Visualizing the pointers L, R, and M is the best way to debug "off-by-one" errors which are common in interview settings.