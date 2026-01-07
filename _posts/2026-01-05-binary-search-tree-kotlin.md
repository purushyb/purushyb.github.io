---
title: "Binary Search Trees: Efficient Lookups in Kotlin"
date: 2026-01-05
categories: [Algorithms, Binary  Search Trees]
tags: [dsa, kotlin, bst, binary-search-tree, algorithms]
math: true
---

A **Binary Search Tree (BST)** is a special type of binary tree that maintains a strict order. This order allows us to search for elements in $O(h)$ time, where $h$ is the height of the tree. In a balanced tree, this becomes $O(\log n)$, making it incredibly fast for large datasets.

## The BST Rule
For every node in a BST:
1. All nodes in the **left subtree** have a value **less than** the node's value.
2. All nodes in the **right subtree** have a value **greater than** the node's value.



---

## Searching in a BST

The search process is very similar to **Binary Search on an array**. Because the tree is ordered, we can discard half of the tree at every step.

### The Algorithm:
1. Compare the `key` with the current node's `data`.
2. If they are equal, we found it!
3. If the `key` is **smaller**, move to the left child.
4. If the `key` is **larger**, move to the right child.
5. Repeat until the key is found or we hit a `null` node.

```kotlin
fun searchElementInBinarySearchTree(head: BinaryTreeNode?, key: Int): Boolean {
    // Base Case: Not found
    if (head == null) return false

    // Base Case: Found it!
    if (head.data == key) return true

    // Recursive Step: Decide which half to search
    return if (key < head.data) {
        searchElementInBinarySearchTree(head.left, key)
    } else {
        searchElementInBinarySearchTree(head.right, key)
    }
}
```

## Implementation Example
Here is how you would construct a simple BST and perform a search:

```kotlin
fun main() {
    // Constructing a BST
    //       6
    //      / \
    //     2   8
    //        / \
    //       7   9
    val head = BinaryTreeNode(6)
    head.left = BinaryTreeNode(2)
    head.right = BinaryTreeNode(8)
    head.right?.left = BinaryTreeNode(7)
    head.right?.right = BinaryTreeNode(9)

    val result = searchElementInBinarySearchTree(head, 9)
    println("Does 9 exist in the BST? $result") // Output: true
}
```

## Complexity Analysis

| Case | Time Complexity | Space Complexity |
| -- | -- | -- |
| Best Case | _O(1)_ (Root is the key) | _O(1)_ |
| Average Case | _O(log n)$_ | _O(h)_ (Stack depth) |
| Worst Case | _O(n)_ (Skewed tree) | _O(n)_ |

Note: The worst case occurs when the tree becomes "skewed" (looking like a linked list). To prevent this, developers use self-balancing trees like AVL or Red-Black trees. {: .prompt-warning }

## Summary

The Binary Search Tree is a powerful structure for maintaining sorted data that needs to be searched frequently. By leveraging the left-is-smaller, right-is-larger rule, we turn a linear search into a logarithmic one.

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/DSABinarySearchTrees.kt).
{: .prompt-tip }