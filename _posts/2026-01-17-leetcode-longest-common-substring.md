---
title: "LeetCode 3: Longest Substring Without Repeating Characters"
date: 2026-01-17
description: "Solving the classic Longest Substring problem using the optimized Sliding Window technique with a HashMap for O(n) time complexity."
categories: [LeetCode, Longest Common Substring]
tags: [leetcode, strings, sliding-window, hashmap, kotlin, medium]
math: true
---

**Longest Substring Without Repeating Characters** is one of the most frequently asked questions in coding interviews. It tests your ability to handle string manipulation efficiently using the **Sliding Window** pattern.

## The Problem
Given a string `s`, find the length of the longest substring without repeating characters.

**Example:**
```kotlin
Input: s = "abcabcbb"
Output: 3
// The answer is "abc", with the length of 3.
```

```
Input: s = "pwwkew"
Output: 3
// The answer is "wke", with the length of 3.
// Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## The Concept: Optimized Sliding Window

A naive solution would check every possible substring, resulting in $O(n^3)$ or $O(n^2)$ complexity. We can do this in linear time $O(n)$ using a sliding window.

Imagine a window defined by `[leftPointer, rightPointer]`. We expand the `rightPointer` one step at a time.

- If the new character is unique, we expand the window and update the max length.

- If the new character is a duplicate, we must contract the window from the left until the duplicate is removed.

### The Optimization

Instead of moving the `leftPointer` one step at a time, we can use a HashMap to store the last seen index of every character. When we encounter a duplicate, we can instantly jump the `leftPointer` to `lastSeenIndex + 1`.

Crucial Check: We must use maxOf when moving leftPointer to ensure we never move it backwards (in case the duplicate we found is actually outside our current window).

## The Solution

```kotlin
object LeetCode_3_LCS_Solution {
    fun lengthOfLongestSubstring(s: String): Int {
        // Map to store Character -> Last Seen Index
        val map = mutableMapOf<Char, Int>()
        
        var maxLength = 0
        var leftPointer = 0

        s.forEachIndexed { rightPointer, currChar ->
            
            // If duplicate found, jump left pointer to (last_index + 1)
            // ensuring we don't go backward
            leftPointer = maxOf(leftPointer, map.getOrDefault(currChar, -1) + 1)

            // Update map with current index
            map[currChar] = rightPointer

            // Calculate window size
            maxLength = maxOf(maxLength, rightPointer - leftPointer + 1)
        }

        return maxLength
    }
}
```

### Usage Example

```kotlin
fun main() {
    val input1 = "abcabcbb"
    println(LeetCode_3_LCS_Solution.lengthOfLongestSubstring(input1)) // Output: 3

    val input2 = "bbbbb"
    println(LeetCode_3_LCS_Solution.lengthOfLongestSubstring(input2)) // Output: 1

    val input3 = "pwwkew"
    println(LeetCode_3_LCS_Solution.lengthOfLongestSubstring(input3)) // Output: 3
}
```

## Complexity Analysis

| Metric | Complexity| Explanation |
|---|---|---| 
|**Time**| $O(n)$ |We iterate through the string exactly once.|
|**Space**|$O(min(n, m))$|We store unique characters in the map. $m$ is the size of the charset (e.g., 26 for a-z, 128 for ASCII)|

## Summary
The key to solving this problem efficiently is knowing when to shrink the window. By tracking indices in a HashMap, we turn a potentially slow "shrink" operation into an instant $O(1)$ jump.

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_3_LCS_Solution.kt).
{: .prompt-tip }