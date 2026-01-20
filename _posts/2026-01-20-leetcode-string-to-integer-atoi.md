---
title: "LeetCode 8: String to Integer (atoi)"
date: 2026-01-20
description: "Implementing the classic C++ atoi function in Kotlin, with a focus on handling edge cases like overflow, whitespace, and signs."
categories: [LeetCode, AtoI]
tags: [leetcode, strings, math, kotlin, medium]
math: true
---

The **String to Integer (atoi)** problem is less about complex algorithms and more about meticulous handling of edge cases. It asks us to implement a function that converts a string to a 32-bit signed integer, mimicking the behavior of the standard C/C++ `atoi` function.

In this post, we'll walk through a clean Kotlin implementation that handles whitespace, signs, and integer overflow.

## The Problem
Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer.

**The algorithm must follow these steps:**
1.  **Whitespace:** Read in and ignore any leading whitespace.
2.  **Sign:** Check if the next character (if not already at the end of the string) is `'-'` or `'+'`. Assume the result is positive if neither is present.
3.  **Conversion:** Read in next the characters until the next non-digit character or the end of the input is reached.
4.  **Overflow:** If the integer is out of the 32-bit signed integer range $[-2^{31}, 2^{31} - 1]$, clamp the integer so that it remains in the range.

**Example:**
```kotlin
Input: s = "   -42 with words"
Output: -42
```

## Approach: Iteration and Validation

The solution is a direct translation of the requirements into code. However, the tricky part is detecting **Integer Overflow** *before* it happens.

### Handling Overflow
We are building the number digit by digit: `result = result * 10 + digit`.
If `result` becomes too large, multiplying it by 10 will cause an overflow.

To prevent this, before updating `result`, we check:
1.  If `result > Int.MAX_VALUE / 10`, we will definitely overflow.
2.  If `result == Int.MAX_VALUE / 10`, we will overflow only if the current `digit` is greater than the last digit of `Int.MAX_VALUE` (which is 7).

### The Code

```kotlin
object LeetCode_8_String_To_Integer {

    fun myAtoi(s: String): Int {
        // Step 1: Handle leading whitespace
        val currStr = s.trimStart()
        if (currStr.isEmpty()) return 0

        var result = 0
        var lp = 0
        var multiplier = 1

        // Step 2: Handle sign
        if (currStr[lp] == '-') {
            multiplier = -1
            lp++
        } else if (currStr[lp] == '+') {
            multiplier = 1
            lp++
        }

        // Step 3 & 4: Process digits and check overflow
        while (lp < currStr.length) {
            val char = currStr[lp]

            // Stop at first non-digit
            if (char < '0' || char > '9') {
                break
            }

            val digit = char - '0'

            // Check for overflow BEFORE adding the digit
            // Int.MAX_VALUE = 2147483647
            if (result > Int.MAX_VALUE / 10 || 
               (result == Int.MAX_VALUE / 10 && digit > Int.MAX_VALUE % 10)) {
                return if (multiplier == 1) Int.MAX_VALUE else Int.MIN_VALUE
            }

            result = (result * 10) + digit
            lp++
        }

        return result * multiplier
    }
}
```

## Complexity Analysis

| Metric | Complexity | Explanation |
|---|---|---|
| **Time** | $O(N)$ | We iterate through the string at most once. |
| **Space** | $O(1)$ | We only use a few variables for tracking the result and index. |

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_8_String_To_Integer.kt).
{: .prompt-tip }
