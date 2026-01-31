---
title: "LeetCode 139: Word Break"
date: 2026-01-21
description: "Solving LeetCode’s Word Break problem in Kotlin, utilizing recursion and memoization to efficiently determine if a string can be segmented into words from a dictionary."
categories: [LeetCode, Word Break]
tags: [recursion, memoization, string manipulation, dynamic-programming]
math: false
---

The **Word Break** problem on LeetCode ([LeetCode Link](https://leetcode.com/problems/word-break/)) asks us to determine if a given string can be segmented into a space-separated sequence of one or more dictionary words. This problem often involves recursion and dynamic programming to avoid redundant calculations, especially when dealing with overlapping subproblems.

In this post, we’ll explore a Kotlin solution that utilizes recursion and memoization to efficiently solve the Word Break problem.

## The Problem
Given a string `s` and a list of strings `wordDict`, determine if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note:**
*   You may use *repeated* characters.  For example, "applepenapple" is segmentable as “apple pen apple”.
*   The dictionary `wordDict` contains only lowercase letters.

**Example:**
```kotlin
Input: s = "leetcode", wordDict = ["leet","code"]
Output: true

Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true
```

## Approach: Recursive Solution with Memoization

The core idea is to use a recursive approach.  Here's how it works:

1. **Base Case:** If the current index `idx` reaches the end of the string (`idx == s.length`), it means we have successfully segmented the entire string, so return `true`.
2. **Memoization:** Use a memoization table (`memo`) to store the results of subproblems. This prevents redundant calculations and significantly improves performance, especially for longer strings.
3. **Iteration:** Iterate through the string from the current index `idx` up to the end of the string.
   - For each character, build a substring (`currString`) from `idx` to the current index.
   - If the `currString` is present in the dictionary (`wordDict`), recursively call `wordBreakRecur` with the remaining part of the string (starting from `idx + 1`).
   - If the recursive call returns `true`, it means we can segment the string, so return `true`.
4. **Memoization Update:** If a particular index has already been computed (memo[idx] != null), return the stored result.
5. **Memoization Failure:** If we cannot segment the string from the current index, set `memo[idx]` to `false` and return `false`.

## The Code

```kotlin
object LeetCode_139_Word_Break {

    fun wordBreak(s: String, wordDict: List<String>): Boolean {
        val wordsSet = HashSet<String>(wordDict)
        return wordBreakRecur(s, wordsSet, 0, Array<Boolean?>(s.length) { null })
    }

    fun wordBreakRecur(s: String, wordsDict: HashSet<String>, idx: Int, memo: Array<Boolean?>): Boolean {
        if(idx == s.length) return true

        if(memo[idx] != null) return memo[idx]!!

        val currString = StringBuilder()

        for(i in idx..<s.length){
            currString.append(s[i])

            if (wordsDict.contains(currString.toString())) {
                if (wordBreakRecur(s, wordsDict, i + 1, memo)) {
                    memo[idx] = true
                    return memo[idx]!!
                }
            }
        }

        memo[idx] = false
        return memo[idx]!!
    }
}

fun main() {
    val s = "leetcode"
    val wordsDict = listOf("leet","code")

    println(LeetCode_139_Word_Break.wordBreak(s, wordsDict))
}
```

## Complexity Analysis

| Metric | Complexity | Explanation |
|---|---|---|
| **Time** | $O(n^2)$ in the worst case (where n is length of s) - due to the nested loop and string concatenation. Memoization helps, but in the worst case it can still be exponential. |
| **Space** | $O(n)$ - for the memoization table and string building. |

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_139_Word_Break.kt).
{: .prompt-tip }

---