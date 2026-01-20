---
title: "LeetCode 383: Ransom Note"
date: 2026-01-20
description: "A simple and efficient solution to verify if a ransom note can be constructed from a magazine using HashMaps in Kotlin."
categories: [LeetCode, Ransome note]
tags: [leetcode, strings, hashmap, kotlin, easy]
math: true
---

The **Ransom Note** problem is a classic example of frequency counting. It asks whether we can construct a specific string (`ransomNote`) using the characters available in another string (`magazine`). Each character in the `magazine` can only be used once.

In this post, we'll solve LeetCode 383 using a **HashMap** to track character counts.

## The Problem
Given two strings `ransomNote` and `magazine`, return `true` if `ransomNote` can be constructed by using the letters from `magazine` and `false` otherwise.

**Example:**
```kotlin
Input: ransomNote = "aa", magazine = "aab"
Output: true
```
```kotlin
Input: ransomNote = "aa", magazine = "ab"
Output: false
```

## Approach: Frequency Counting

The core logic is simple: for every character needed in the `ransomNote`, we must have a corresponding "available" character in the `magazine`.

1.  **Count Availability**: Iterate through the `magazine` string and count the frequency of each character. A `HashMap<Char, Int>` is perfect for this.
2.  **Consume Characters**: Iterate through the `ransomNote`. For each character:
    *   Check if it exists in our map with a count $> 0$.
    *   If yes, decrement the count (use up the character).
    *   If no (or count is 0), return `false` immediately because we don't have enough letters.
3.  **Success**: If we finish checking the `ransomNote` without running out of characters, return `true`.

### The Code

Here is the Kotlin implementation using a `HashMap`:

```kotlin
object LeetCode_383_Ransome_Note {

    fun canConstruct(ransomNote: String, magazine: String): Boolean {
        val table = HashMap<Char, Int>()

        // Step 1: Build frequency map of the magazine
        for(char in magazine) {
            table[char] = table.getOrDefault(char, 0) + 1
        }

        // Step 2: Check if ransomNote can be formed
        for(char in ransomNote) {
            val count = table.getOrDefault(char, 0)
            
            if (count == 0) {
                return false // Not enough characters
            } else {
                table[char] = count - 1 // Use one character
            }
        }

        return true
    }
}
```

### Optimization Tip
While a `HashMap` is a great general-purpose solution, if we know the input strings only contain lowercase English letters, we can use an **Integer Array** of size 26 instead. This avoids the overhead of hashing and is generally faster.

```kotlin
// Array-based approach (Space Optimization)
val counts = IntArray(26)
for (c in magazine) counts[c - 'a']++
for (c in ransomNote) {
    if (counts[c - 'a'] == 0) return false
    counts[c - 'a']--
}
```

## Complexity Analysis

| Metric | Complexity | Explanation |
|---|---|---|
| **Time** | $O(m + n)$ | We iterate through the `magazine` ($m$) once and the `ransomNote` ($n$) once. |
| **Space** | $O(1)$ | Although we use a map, the number of unique characters is constant (e.g., 26 lowercase English letters), so the space complexity is technically constant. |

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_383_Ransome_Note.kt).
{: .prompt-tip }
