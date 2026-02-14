---

title: "LeetCode 17: Letter Combinations of Phone Numbers"
date: 2026-02-14
description: "Letter Combinations of Phone Numbers, in Kotlin. We’ll explore two approaches: a straightforward iterative solution and a more advanced Breadth-First Search (BFS) style approach."
categories: [LeetCode, String Manipulation, Graph Algorithms]
tags: [bfs, oranges, rotting, graph, algorithm]
math: false

---

The LeetCode 17 problem ("Letter Combinations of Phone Numbers") challenges you to generate all possible letter combinations for a given digits string. For example, for the input “23”, you should return ["ad", "ae", "ab", "bd", "be", "bb"].

## The Problem

Given a digits string `digits`, return *all* possible letter combinations that can be formed from this digit input.

Each digit on its own corresponds to its letters as defined by `digitsMap`.
Note that each letter in the answer should be sorted alphabetically.

**Example:**

```kotlin
// Example:
// digits = "23"
// Output: ["ad", "ae", "ab"]
```

## Approach 1: Iterative Solution (String Manipulation)

This approach uses a straightforward string manipulation technique. It’s relatively easy to understand and implement, but can become less efficient for longer digit strings due to the nested loops.

1.  **Digit Map:** We define a `digitsMap` array, where each index corresponds to a digit (0-9), and the value is a string of letters associated with that digit.
2.  **Iterate Through Digits:** We iterate through each digit in the input `digits` string.
3.  **Generate Combinations:** For each digit, we retrieve the corresponding letters from `digitsMap`. We then iterate through all possible prefixes (combinations) built so far and append each letter from the current digit's letters to create new combinations.
4.  **Return Results:** Finally, we return the list of generated letter combinations.

## The Code (Kotlin - Iterative)

```kotlin
object LeetCode_17_Letter_Combination_Phone_number {
    fun letterCombinations(digits: String): List<String> {
        val digitsMap = arrayOf("abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz")
        var results = listOf<String>("")

        for (digit in digits) {
            val currLetters = digitsMap[digit - '2']
            val temp = mutableListOf<String>()

            for (prefix in results) {
                for (letter in currLetters) {
                    temp.add(prefix + letter)
                }
            }

            results = temp
        }
        return results
    }
}

fun main() {
    val digits = "23"
    println(LeetCode_17_Letter_Combination_Phone_number.letterCombinations(digits))
}
```

## Approach 2: BFS-Style Solution (Graph Traversal)

This approach utilizes Breadth-First Search (BFS), treating the digit combinations as a graph.  It can be more efficient for longer digit strings, especially when considering the potential for overlapping combinations.

1.  **Digit Map:** Same as before, we define `digitsMap`.
2.  **Queue Initialization:** We initialize a queue with an empty string (""). This represents the initial state (no letters chosen).
3.  **BFS Loop:** We iterate through each digit in the input `digits` string.
4.  **Dequeue and Extend:** Inside the loop, we dequeue a prefix from the queue. For each letter in the current digit's letters:
    *   We append that letter to the prefix and enqueue the new combination.
5.  **Return Results:** After processing all digits, we return the queue containing all possible letter combinations.

## The Code (Kotlin - BFS)

```kotlin
object LeetCode_17_Letter_Combination_Phone_number {
    fun letterCombinationsBFSStyle(digits: String): List<String> {
        val digitsMap = arrayOf("abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz")

        val queue = LinkedList<String>()

        queue.offer("")

        for (currLevel in 0..<digits.length) {
            val currPrefixIndex = digits[currLevel] - '2'

            while(queue.peek().length == currLevel){
                val currPrefix = queue.poll()
                for (i in digitsMap[currPrefixIndex]) {
                    queue.add(currPrefix + i)
                }
            }
        }

        return queue
    }
}

fun main() {
    val digits = "23"
    println(LeetCode_17_Letter_Combination_Phone_number.letterCombinationsBFSStyle(digits))
}
```

## Complexity Analysis

| Metric | Iterative Solution | BFS-Style Solution |
|---|---|---|
| **Time** | $O(N * 8^N)$ (where N is the length of `digits`) -  The nested loops can become computationally expensive for longer digit strings. | $O(N * 8^N)$ -  The time complexity remains the same, but BFS can be more efficient in practice due to its ability to avoid redundant calculations. |
| **Space** | $O(N)$ -  The space complexity is dominated by the `results` list, which can grow to a size proportional to $8^N$. | $O(N * 8^N)$ - The space complexity is dominated by the queue, which can grow to a size proportional to $8^N$. |

## Conclusion

Both solutions achieve the desired outcome. The iterative approach is simpler to understand, while the BFS-style solution can be more efficient for longer digit strings and offers a different perspective on solving this problem.  Choose the approach that best suits your needs based on readability and performance considerations.

---