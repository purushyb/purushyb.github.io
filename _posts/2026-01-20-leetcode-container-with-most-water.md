---
title: "LeetCode 11: Container With Most Water"
date: 2026-01-20
description: "Optimizing area calculations with the efficient Two Pointer technique in Kotlin."
categories: [LeetCode, Container with most water]
tags: [leetcode, arrays, two-pointers, greedy, kotlin, medium]
math: true
---

**Container With Most Water** is a famous problem that demonstrates the power of the **Two Pointer** pattern. A brute-force solution checking every pair of lines would take $O(n^2)$ time, but by making greedy choices, we can solve it in linear time.

In this post, we'll implement the $O(n)$ solution.

## The Problem
You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the $i^{th}$ line are $(i, 0)$ and $(i, height[i])$.

Find two lines that together with the x-axis form a container, such that the container contains the most water. Return the maximum amount of water a container can store.

**Example:**
```kotlin
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
// Explanation: The vertical lines are at indices 1 and 8 (values 8 and 7).
// Area = min(8, 7) * (8 - 1) = 7 * 7 = 49
```

## Approach: Two Pointers

The area of water is determined by two factors:
1.  **Width:** The distance between the two lines (`rp - lp`).
2.  **Height:** The height of the shorter line (`min(height[lp], height[rp])`).

$$ \text{Area} = \min(\text{height}[lp], \text{height}[rp]) \times (rp - lp) $$

To maximize the area, we want both width and height to be as large as possible.

### The Strategy
1.  **Start Max Width**: Place one pointer at the beginning (`lp = 0`) and one at the end (`rp = n-1`). This gives us the maximum possible width.
2.  **Greedy Move**: To find a larger area, we must try to increase the height (since the width will only decrease as we move pointers inward).
    *   The area is limited by the **shorter** line.
    *   If we move the pointer at the taller line, the width decreases, and the height is still limited by the shorter line (or becomes even shorter). The area is guaranteed to decrease or stay the same.
    *   Therefore, we must move the pointer at the **shorter** line inward, hoping to find a taller line that compensates for the lost width.

### The Code

```kotlin
object LeetCode_11_Container_With_Most_Water {
    fun maxArea(height: IntArray): Int {
        var lp = 0
        var rp = height.size - 1
        var maxArea = 0

        while (lp < rp) {
            // Calculate current area
            // Area = min height * width
            val currArea = minOf(height[lp], height[rp]) * (rp - lp)

            if (currArea > maxArea) maxArea = currArea

            // Move the shorter line inward
            if (height[lp] > height[rp]) {
                rp--
            } else {
                lp++
            }
        }

        return maxArea
    }
}
```

## Complexity Analysis

| Metric | Complexity | Explanation |
|---|---|---|
| **Time** | $O(n)$ | We touch each element of the array at most once (pointers only move inward). |
| **Space** | $O(1)$ | We only use a few variables for indices and area tracking. |

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_11_Container_With_Most_Water.kt).
{: .prompt-tip }
