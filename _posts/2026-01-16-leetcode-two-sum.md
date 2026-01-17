---
title: "LeetCode 1: Two Sum"
date: 2026-01-16
description: "A detailed walkthrough of the classic Two Sum problem, moving from a brute-force approach to an optimized O(n) solution using HashMaps."
categories: [LeetCode, Two sum]
tags: [leetcode, arrays, hashmap, kotlin, easy]
math: true
---

The **Two Sum** problem is the "Hello World" of technical interviews. While it seems simple, moving from the naive solution to the optimal one demonstrates your understanding of Time vs. Space complexity trade-offs.

## The Problem
Given an array of integers `nums` and an integer `target`, return the *indices* of the two numbers such that they add up to `target`.

**Constraints:**
* You may assume that each input would have **exactly one solution**.
* You may not use the same element twice.

**Example:**
```kotlin
Input: nums = [2, 7, 11, 15], target = 9
Output: [0, 1]
// Because nums[0] + nums[1] == 9 (2 + 7 = 9)
```

## Approach 1: Brute Force (Naive)

he most intuitive approach is to check every pair of numbers. We can loop through each element x and then loop through every other element `y` to see if `x + y == target`.

- Time Complexity: $O(n^2)$ — For each element, we scan the rest of the array.
- Space Complexity: $O(1)$ — No extra data structures needed.

While this works, we can do better.

## Approach 2: One-Pass HashMap (Optimized)

Instead of scanning the array repeatedly to find the "complement" (the number we need to complete the sum), we can remember the numbers we've seen so far.

We iterate through the array once. For every number `currNum`, we calculate the `required` number:

$$required = target - currNum$$

We then check our HashMap:

- Is `required` in the map? If yes, we found our pair! Return the index of `required` (from the map) and the current index.

- If no: Store `currNum` and its index `i` in the map and continue.

The Code

```kotlin
object LeetCode_1_TwoSum_Solution {
    fun twoSum(nums: IntArray, target: Int): IntArray {
        // Map to store number -> index
        val map = mutableMapOf<Int, Int>()

        nums.forEachIndexed { i, currNum ->
            val required = target - currNum

            // Check if the complement exists in our map
            val targetIndex = map.getOrDefault(required, -1)
            
            if (targetIndex != -1) {
                // Found it! Return both indices
                return intArrayOf(targetIndex, i)
            }

            // Store current number and index for future lookups
            map[currNum] = i
        }

        throw Exception("No solution found")
    }
}
```

### Usage Example

```kotlin
fun main() {
    val input1 = intArrayOf(2, 7, 11, 15)
    val target1 = 9
    println(LeetCode_1_TwoSum_Solution.twoSum(input1, target1).contentToString()) 
    // Output: [0, 1]

    val input2 = intArrayOf(3, 2, 4)
    val target2 = 6
    println(LeetCode_1_TwoSum_Solution.twoSum(input2, target2).contentToString()) 
    // Output: [1, 2]
}
```

## Complexity Analysis

| Approach | Time Complexity | Space Complexity |
| --- | --- | --- |
|**Brute Force**| $O(n^2)$| $O(1)$ |
|**HashMap**| $O(n)$ (Nearly constant)| $O(n)$ |

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_1_TwoSum_Solution.kt.kt).
{: .prompt-tip }