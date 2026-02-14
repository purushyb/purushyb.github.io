---
title: "LeetCode 994: Rotting Oranges"
date: 2026-02-14
description: "Solving LeetCode's Rotting Oranges problem in Kotlin, using Breadth-First Search (BFS) to efficiently simulate the rotting of oranges and determine the minimum time required."
categories: [LeetCode, Breadth-First Search, Linked Lists]
tags: [bfs, oranges, rotting, graph, algorithm]
math: false
---

The **Rotting Oranges** problem on LeetCode ([LeetCode Link](https://leetcode.com/problems/rotting-oranges/)) presents a fascinating challenge involving simulating the spread of rotting oranges in a grid. You need to determine the minimum time it takes for all rotten oranges to rot and reach their neighbors. This blog post will walk through a Kotlin solution utilizing Breadth-First Search (BFS).

## The Problem

You are given a 2D grid representing oranges where:

*   `0`: Represents an empty cell.
*   `1`: Represents a fresh orange.
*   `2`: Represents a rotten orange.

Oranges rot at time `t`. In one minute, each rotten orange will rot all its four neighboring oranges (up, down, left, right).

Return the minimum time required for all fresh oranges to rot. If it is impossible for all oranges to rot, return -1.

**Example:**

```kotlin
// Example:
// 2 1 1
// 1 1 0
// 0 1 1

val grid = arrayOf(intArrayOf(2, 1, 1), intArrayOf(1, 1, 0), intArrayOf(0, 1, 1))

println(LeetCode_994_Rotting_Oranges.orangesRotting(grid)) // Output: 2
```

## Approach: Breadth-First Search (BFS)

The most suitable approach for this problem is **Breadth-First Search (BFS)**. BFS systematically explores the grid layer by layer, ensuring that we process oranges closer to the source (rotten oranges) before those further away.

1.  **Initialization:** We start by initializing a queue (`q`) with all rotten oranges and a `freshOranges` counter to track the number of fresh oranges.
2.  **BFS Loop:** We iterate as long as the queue is not empty:
    *   **Process Current Level:**  We process all oranges at the current level (size of queue). For each rotten orange, we explore its four neighbors.
    *   **Rot Neighbors:** If a neighbor is a fresh orange (`grid[newR][newC] == 1`), we mark it as rotten (`grid[newR][newC] = 2`), decrement `freshOranges`, and add it to the queue.
    *   **Increment Time:** If we processed any oranges in this level (meaning a rotten orange had neighbors to rot), we increment the `minute` counter.
3.  **Check for Completion:** After the BFS loop finishes, if `freshOranges` is 0, it means all oranges have rotted, and we return the `minute`. Otherwise, if there are still fresh oranges remaining, it means it's impossible to rot all oranges, and we return -1.

## The Code (Kotlin)

```kotlin
object LeetCode_994_Rotting_Oranges {

    fun orangesRotting(grid: Array<IntArray>): Int {
        val q = LinkedList<Pair<Int, Int>>()
        val rowSize = grid.size
        val colSize = grid.first().size
        var freshOranges = 0

        // find all rotted oranges and add it to queue
        for (row in 0..<rowSize) {
            for (col in 0..<colSize) {
                if (grid[row][col] == 2) {
                    q.offer(Pair(row, col))
                }
                else if (grid[row][col] == 1) {
                    freshOranges++
                }
            }
        }

        if(freshOranges == 0) return 0

        var minute = 0

        val directions = arrayOf(
            intArrayOf(-1, 0), intArrayOf(1, 0),
            intArrayOf(0, -1), intArrayOf(0, 1)
        )

        // Multi BFS
        while (q.isNotEmpty()) {
            val size = q.size // FREEZE the size for this minute level
            var infectedThisRound = false

            for (i in 0 until size) {
                val (currR, currC) = q.poll()

                for (d in directions) {
                    val newR = currR + d[0]
                    val newC = currC + d[1]

                    // Check bounds and if fresh
                    if (newR >= 0 && newR < rowSize &&
                        newC >= 0 && newC < colSize &&
                        grid[newR][newC] == 1) {

                        // Rot the orange
                        grid[newR][newC] = 2
                        freshOranges--
                        q.offer(Pair(newR, newC))
                        infectedThisRound = true
                    }
                }
            }

            if (infectedThisRound) minute++
        }
        return if(freshOranges == 0) minute else -1
    }
}

fun main() {
    val grid = arrayOf(intArrayOf(2, 1, 1), intArrayOf(1, 1, 0), intArrayOf(0, 1, 1))

    println(LeetCode_994_Rotting_Oranges.orangesRotting(grid))
}
```

## Complexity Analysis

| Metric | Complexity | Explanation |
|---|---|---|
| **Time** | $O(M * N)$ | Where M is the number of rows and N is the number of columns in the grid.  The BFS explores each cell at most once. |
| **Space** | $O(M * N)$ | In the worst case, the queue could contain all cells in the grid.  This is because every cell can be a rotten orange and added to the queue. |

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_994_Rotting_Oranges.kt).
{: .prompt-tip }

---