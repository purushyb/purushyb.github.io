---
title: "Recursion: From Basics to The Knapsack Problem"
date: 2026-01-12
description: "A comprehensive guide to understanding recursive algorithms, covering Factorials, Fibonacci, and complex decision trees like the 0/1 Knapsack problem."
categories: [General Algorithms, Recursion]
tags: [dsa, kotlin, recursion, knapsack, lcs, dynamic-programming]
math: true
---

Recursion is a method where the solution to a problem depends on solutions to smaller instances of the same problem. Every recursive function needs two things:
1.  **Base Case:** The condition that stops the recursion.
2.  **Recursive Case:** The logic that breaks the problem down into a smaller version of itself.

In this post, we will explore 5 examples, ranging from simple math to classic interview questions.

---

## 1. The Warm-ups: Sum and Factorial

The simplest recursive functions are linear—they call themselves once.

### Sum of N
Calculating the sum of numbers from 1 to $N$.
* **Base:** If $n=1$, return 1.
* **Recurse:** Return $n + sum(n-1)$.

```kotlin
fun sum(n: Int): Int {
    if (n == 1) return n
    return n + sum(n - 1)
}
```

### Factorial

Calculating $N!$ (e.g., $5! = 5 \times 4 \times 3 \times 2 \times 1$).
- Base: If $n=1$, return 1.
- Recurse: Return $n \times fact(n-1)$.

```kotlin
fun fact(n: Int): Int {
    if (n == 1) return n
    return n * fact(n - 1)
}
```

## 2. Branching Recursion: Fibonacci

Things get interesting when a function calls itself multiple times. The Fibonacci sequence is the classic example.

```kotlin
fun fib(n: Int): Int {
    if (n == 1 || n == 0) return n
    return fib(n - 1) + fib(n - 2)
}
```
Note: Without optimization (Memoization), this algorithm is $O(2^n)$ because it re-calculates the same values repeatedly.

## 3. The 0/1 Knapsack Problem

This is a classic optimization problem. Given a set of items, each with a weight and a profit, determine the maximum profit you can generate for a given weight capacity.

### The Strategy: "Pick or Don't Pick"

For every item, we have two choices:

1. Pick it: Add its profit to the total, subtract its weight from the capacity, and move to the next item.
2. Don't Pick it: Skip the item and keep the capacity the same.

We calculate both paths and take the `maxOf`.

```kotlin
fun knapSackRecur(weights: Array<Int>, profits: Array<Int>, currWeight: Int, position: Int): Int {
    // Base case: No items left or no capacity left
    if (position == 0 || currWeight == 0) return 0

    var pickedProfit = 0
    val currPosition = position - 1

    // Option 1: Pick the item (if it fits)
    if (weights[currPosition] <= currWeight) {
        pickedProfit = profits[currPosition] + knapSackRecur(
            weights,
            profits,
            currWeight - weights[currPosition],
            currPosition
        )
    }

    // Option 2: Don't pick the item
    val notPickedProfit = knapSackRecur(weights, profits, currWeight, currPosition)

    // Return the better outcome
    return maxOf(pickedProfit, notPickedProfit)
}
```

## 4. Longest Common Subsequence (LCS)

Given two strings (e.g., "AGGTAB" and "GXTXAYB"), find the length of the longest subsequence present in both.

### The Logic

We compare the characters at the current `position1` and `position2`.

1. Match: If chars are equal, add 1 to the result and move both pointers back.
2. No Match: We have two choices—move pointer 1 back OR move pointer 2 back. We take the `max` of these two paths.

```kotlin
fun longestCommonSubsequence(sequence1: String, sequence2: String, position1: Int, position2: Int): Int {
    // Base Case
    if (position1 == 0 || position2 == 0) return 0

    if (sequence1[position1] == sequence2[position2]) {
        // Match found: Add 1 and move diagonal
        return 1 + longestCommonSubsequence(
            sequence1,
            sequence2,
            position1 - 1,
            position2 - 1
        )
    } else {
        // No match: Try both branches
        return maxOf(
            longestCommonSubsequence(sequence1, sequence2, position1 - 1, position2),
            longestCommonSubsequence(sequence1, sequence2, position1, position2 - 1)
        )
    }
}
```

## Summary

Recursion allows us to solve complex problems by breaking them down into simple decision trees.

- Linear Recursion: Sum, Factorial.
- Branching Recursion: Fibonacci.
- Decision Trees: Knapsack (Pick/Don't Pick), LCS (Match/Skip).

While these recursive solutions are clean and correct, they can be slow for large inputs due to overlapping subproblems. This is where Dynamic Programming comes in (which we will cover next!).

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/GeneralTechniquesRecursion.kt).
{: .prompt-tip }