---
title: "LeetCode 5: Longest Palindromic Substring"
date: 2026-01-18
description: "Comparing two powerful solutions for finding palindromes: the space-efficient 'Expand Around Center' and the classic Dynamic Programming approach."
categories: [LeetCode, Longest Palindromic Substring]
tags: [leetcode, strings, dynamic-programming, palindrome, kotlin, medium]
math: true
---

Finding the **Longest Palindromic Substring** is a staple dynamic programming question. However, standard DP isn't always the most efficient way to solve it in terms of memory.

In this post, we explore two solutions:
1.  **Expand Around Center** (Efficient Space: $O(1)$)
2.  **Dynamic Programming** (Standard Approach: $O(n^2)$ Space)

## The Problem
Given a string `s`, return the longest palindromic substring in `s`.

**Example:**
```kotlin
Input: s = "babad"
Output: "bab" 
// "aba" is also a valid answer.
```

## Approach 1: Expand Around Center

A palindrome mirrors around its center. Therefore, a palindrome can be expanded from its middle.

- Odd length palindromes (e.g., "aba") have one center character.
- Even length palindromes (e.g., "abba") have two center characters.

We iterate through each character in the string and treat it as a center, expanding outwards as long as the characters match.

### The Code

The inner loop `for (j in 0..1)` cleverly handles both odd `(low == high)` and even `(high = low + 1)` centers in one pass.

```kotlin
fun longestPalindromeEvenOddLength(s: String): String {
    val strSize = s.length
    var maxSize = 0
    var startIndex = 0

    for (i in 0 until strSize) {
        // j=0 checks odd length (center is i)
        // j=1 checks even length (center is i, i+1)
        for (j in 0..1) {
            var low = i
            var high = low + j

            // Expand as long as boundary is valid and chars match
            while (low >= 0 && high < strSize && s[low] == s[high]) {
                val currSize = high - low + 1
                if (maxSize < currSize) {
                    startIndex = low
                    maxSize = currSize
                }
                low--
                high++
            }
        }
    }

    return s.substring(startIndex, startIndex + maxSize)
}
```

## Approach 2: Dynamic Programming

We can build a 2D table `dp[i][j]` where the value is `1` (true) if the substring `s[i...j]` is a palindrome, and 0 otherwise.

The logic relies on the fact that `s[i...j]` is a palindrome ONLY if:

1. The outer characters match `(s[i] == s[j])`.
2. The inner substring `s[i+1...j-1]` is already a known palindrome.

### The Code

We fill the table diagonally. First length 1, then length 2, then length 3, and so on.

```kotlin
fun longestPalindromeDP(s: String): String {
    val strSize = s.length
    // dp[i][j] = 1 means substring i..j is a palindrome
    val dp = Array(strSize + 1) { Array<Int>(strSize + 1) { 0 } }
    var maxLength = 0
    var startIndex = 0

    // Step 1: All substrings of length 1 are palindromes
    for (i in 0 until strSize) {
        dp[i][i] = 1
    }
    maxLength = 1

    // Step 2: Check substrings of length 2
    for (i in 0 until strSize - 1) {
        if (s[i] == s[i + 1]) {
            dp[i][i + 1] = 1
            startIndex = i
            maxLength = 2
        }
    }

    // Step 3: Check lengths 3 and greater
    for (len in 3..strSize) {
        for (j in 0..strSize - len) {
            val k = j + len - 1 // Ending index
            
            // Check outer chars AND inner substring
            if (s[j] == s[k] && dp[j + 1][k - 1] == 1) {
                dp[j][k] = 1
                if (maxLength < len) {
                    startIndex = j
                    maxLength = len
                }
            }
        }
    }

    return s.substring(startIndex, startIndex + maxLength)
}
```

## Complexity Comparison

|Metric | Expand Around Center |Dynamic Programming|
|---|---|---|
|Time| $O(n^2)|$$O(n^2)$|
|Space|$O(1)$|$O(n^2)$|


Why use Expand Around Center?
Although both are $O(n^2)$ time, the DP approach requires allocating a large 2D array. For a string of length 1000, that's $1,000,000$ integers. The expansion method uses constant extra space, making it much more practical for memory-constrained environments.

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_5_Palindrome_Solution.kt).
{: .prompt-tip }