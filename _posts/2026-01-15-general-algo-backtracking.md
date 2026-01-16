---
title: "Backtracking: Solving Sudoku and N-Queens"
date: 2026-01-15
description: "Master the art of Backtracking by building a Sudoku Solver and finding all solutions to the N-Queens problem in Kotlin."
categories: [General Algorithms, Backtracking]
tags: [dsa, kotlin, backtracking, n-queens, sudoku, recursion]
math: true
---

**Backtracking** is an algorithmic technique for solving problems recursively by trying to build a solution incrementally, one piece at a time. The moment we determine that the current path cannot possibly lead to a valid solution, we **backtrack** (remove the last choice) and try a different option.

It's essentially a "Depth-First Search" on the state space of the problem. Today, we'll solve two classic puzzles using this technique.

---

## 1. The Sudoku Solver

A standard Sudoku puzzle consists of a $9 \times 9$ grid. The objective is to fill the grid with digits so that each column, each row, and each of the nine $3 \times 3$ subgrids contain all digits from 1 to 9.

### The Backtracking Strategy
1.  Find the first empty cell (represented by `0`).
2.  Try placing digits `1` through `9`.
3.  Check if the digit is **safe** (not present in the current row, column, or $3 \times 3$ box).
4.  If safe, place it and recursively try to solve the rest of the board.
5.  If the recursive call fails (returns `false`), reset the cell to `0` (**Backtrack**) and try the next digit.



```kotlin
fun sudokuSolverRecur(matrix: Array<Array<Int>>, row: Int, column: Int): Boolean {
    var currRow = row
    var currColumn = column

    // Base Case: Reached end of matrix
    if (row == 8 && column == 9) return true

    // Move to next row if column limit reached
    if (column == 9) {
        currRow++
        currColumn = 0
    }

    // Skip already filled cells
    if (matrix[currRow][currColumn] != 0) {
        return sudokuSolverRecur(matrix, currRow, currColumn + 1)
    }

    for (i in 1..9) {
        if (isSafeSudoku(matrix, currRow, currColumn, i)) {
            matrix[currRow][currColumn] = i // Place Number
            
            if (sudokuSolverRecur(matrix, currRow, currColumn + 1)) return true
            
            matrix[currRow][currColumn] = 0 // Backtrack
        }
    }
    return false
}
```

## 2. The N-Queens Problem

The N-Queens puzzle involves placing $N$ queens on an $N \times N$ chessboard such that no two queens attack each other. This means no two queens can share the same row, column, or diagonal.

### The Strategy

We place queens row by row.

1. Place a queen in column 0 of the current row.
2. Check if it's under attack from queens in previous rows (Top, Top-Left, Top-Right).
3. If safe, recurse to the next row.
4. If not safe (or if the recursion fails), remove the queen and try column 1.

```kotlin
fun nQueens(matrix: Array<Array<Int>>, output: MutableList<Array<Int>>, row: Int) {
    val queensCount = matrix.size

    // Base case: All queens placed
    if (row == queensCount) {
        val newCombination = Array<Int>(queensCount) { 0 }
        for (i in 0 until queensCount) {
            for (j in 0 until queensCount) {
                if (matrix[i][j] == 1) newCombination[i] = j + 1
            }
        }
        output.add(newCombination)
        return // Or remove to find ALL solutions
    }

    for (i in 0 until queensCount) {
        if (isSafePosition(matrix, row, i)) {
            matrix[row][i] = 1 // Place Queen
            nQueens(matrix, output, row + 1)
            matrix[row][i] = 0 // Backtrack
        }
    }
}
```

## 3. String Permutations

Backtracking is also used to generate permutations. Here, the "choice" is swapping the current character with every other character that follows it.

```kotlin
fun permutationsRecur(str: StringBuilder, index: Int, op: MutableList<String>) {
    if (index == str.length) {
        op.add(str.toString())
        return
    }

    for (i in index until str.length) {
        swapStr(str, index, i)           // Choose
        permutationsRecur(str, index + 1, op) // Explore
        swapStr(str, i, index)           // Un-choose (Backtrack)
    }
}
```

## Summary
Backtracking is a "brute-force with intelligence." Unlike pure brute-force which generates all invalid states, backtracking prunes the search tree as soon as it detects a violation (`isSafe`).

- Sudoku: Constraints are Row, Column, and Subgrid.

- N-Queens: Constraints are Column and Diagonals.

- Permutations: Constraint is effectively "don't reuse index."


## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/GeneralTechniquesBackTrack.md.kt).
{: .prompt-tip }