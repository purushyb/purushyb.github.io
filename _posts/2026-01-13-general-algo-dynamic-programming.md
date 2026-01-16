---
title: "Dynamic Programming: Optimizing Recursion"
date: 2026-01-13
description: "A practical guide to Dynamic Programming (DP), covering Memoization (Top-Down) and Tabulation (Bottom-Up) for Fibonacci, Knapsack, and LCS."
categories: [General Algorithms, Dynamic Programming]
tags: [dsa, kotlin, dynamic-programming, dp, memoization, algorithms]
math: true
---

In our previous post on Recursion, we solved problems like Fibonacci and Knapsack. While the code was clean, it was inefficient. Pure recursion often solves the same subproblems over and over again, leading to exponential time complexity ($O(2^n)$).

**Dynamic Programming (DP)** is simply recursion with a "cache." By storing the results of subproblems, we ensure that each unique input is computed only once.

---

## 1. Fibonacci: Top-Down vs Bottom-Up

There are two main ways to implement DP:
1.  **Top-Down (Memoization):** Keep the recursive logic but add a lookup table.
2.  **Bottom-Up (Tabulation):** Start from the smallest case and iteratively build up to the answer.

[Image of comparison between top-down memoization and bottom-up tabulation for fibonacci]

### Top-Down (Memoization)
We pass a `table` array to store results. Before computing `fib(n)`, we check if `table[n]` already has a value.

```kotlin
// Top-Down: Recursion + Caching
fun fibDPRecur(n: Int, table: Array<Int?>): Int {
    // 1. Table Lookup (Memoization Check)
    if (table[n] != null) return table[n]!!

    // 2. Base Condition
    if (n == 0 || n == 1) {
        table[n] = n
    } else {
        // 3. Compute and Store
        table[n] = fibDPRecur(n - 1, table) + fibDPRecur(n - 2, table)
    }

    return table[n]!!
}
```

### Bottom-Up (Tabulation)
Here, we avoid recursion entirely. We fill the table iteratively from `0` to `n`.

```kotlin
// Bottom-Up: Iterative Filling
fun fibDp(n: Int): Int {
    if (n <= 1) return n

    val table = Array<Int?>(n + 1) { null }
    table[0] = 0
    table[1] = 1

    for (i in 2..n) {
        table[i] = table[i - 1]!! + table[i - 2]!!
    }

    return table[n]!!
}
```

## 2. 0/1 Knapsack with Memoization

In the Knapsack problem, our state depends on two variables: `position` (items left) and `currWeight` (capacity left). Therefore, our memoization table must be a 2D Array.

```kotlin
fun knapSackDpRecur(
    weights: Array<Int>,
    profits: Array<Int>,
    currWeight: Int,
    position: Int,
    memo: Array<Array<Int>>
): Int {
    // Base Case
    if (position == 0 || currWeight == 0) return 0

    // Check Cache
    if (memo[position][currWeight] != -1) {
        return memo[position][currWeight]
    }

    var pickedProfit = 0
    val currPosition = position - 1

    // Logic: Pick vs Don't Pick
    if (weights[currPosition] <= currWeight) {
        pickedProfit = profits[currPosition] + knapSackDpRecur(
            weights,
            profits,
            currWeight - weights[currPosition],
            currPosition,
            memo
        )
    }
    val notPickedProfit = knapSackDpRecur(weights, profits, currWeight, currPosition, memo)

    // Store Result
    val result = maxOf(pickedProfit, notPickedProfit)
    memo[position][currWeight] = result

    return result
}
```

## 3. Longest Common Subsequence (LCS)

Similarly, LCS depends on two indices: `position1` (string 1 index) and `position2` (string 2 index). We cache the result of every `(p1, p2)` pair.

```kotlin
fun longestCommonSubsequenceDp(
    sequence1: String,
    sequence2: String,
    position1: Int,
    position2: Int,
    memo: Array<Array<Int>>
): Int {
    if (position1 == 0 || position2 == 0) return 0

    if (memo[position1][position2] != -1) return memo[position1][position2]

    if (sequence1[position1] == sequence2[position2]) {
        // Match: 1 + result of remaining
        val res = 1 + longestCommonSubsequenceDp(
            sequence1,
            sequence2,
            position1 - 1,
            position2 - 1,
            memo
        )
        memo[position1 - 1][position2 - 1] = res
        return res
    } else {
        // No Match: Try skipping char from either string
        val res = maxOf(
            longestCommonSubsequenceDp(sequence1, sequence2, position1 - 1, position2, memo),
            longestCommonSubsequenceDp(sequence1, sequence2, position1, position2 - 1, memo)
        )
        memo[position1][position2] = res
        return res
    }
}
```

## Complexity Comparison

| Operation | Naive Implementation | With Optimizations |
| --- | --- | --- |
|**Fibonacci**|_$O(2^n)$_|$O(n)$|
|**Knapsack**|_O(N)_|$O(M \times N)$|
|**LCS**|_O(N)_|$O(M \times N)$|

Where $N, M$ are lengths and $W$ is capacity.

## Summary

Dynamic Programming is not magic; it's just Recursion + Memory.
1. Start with a working recursive solution.
2. Identify the changing parameters (state).
3. Add a memo table to store results for those states.
4. Return the stored result if it exists.

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/GeneralTechniquesDynamicProgramming.kt).
{: .prompt-tip }