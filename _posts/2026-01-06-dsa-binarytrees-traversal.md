---
title: "Binary Tree : DFS and BFS Explained"
description: "Binary Tree Traversals in Kotlin: DFS and BFS Explained"
date: 2026-01-04
categories: [Algorithms, Binary tree traversals]
tags: [dsa, kotlin, binary-tree, recursion, bfs, dfs]
math: true
---

Binary Trees are the foundation for many advanced data structures like AVL trees, Red-Black trees, and Heaps. To work with them effectively, you must master the two primary ways to visit every node: **Depth-First Search (DFS)** and **Breadth-First Search (BFS)**.

## The Foundation: BinaryTreeNode
In Kotlin, we represent a tree node using a data class with nullable references for the left and right children.



```kotlin
data class BinaryTreeNode(
    val data: Int, 
    var left: BinaryTreeNode? = null, 
    var right: BinaryTreeNode? = null
)
```

## 1. Depth-First Search (DFS)

DFS uses recursion to go as deep as possible down one branch before backing up. There are three classic DFS orders:

Pre-Order (Root -> Left -> Right)

Perfect for creating a copy of a tree.

```kotlin
fun dfsPrintPreOrder(node: BinaryTreeNode?) {
    if (node == null) return
    print("${node.data} ")
    dfsPrintPreOrder(node.left)
    dfsPrintPreOrder(node.right)
}
```

## 2. In-Order (Left -> Root -> Right)
If used on a Binary Search Tree (BST), this returns the values in sorted order.

```kotlin
fun dfsPrintInOrder(node: BinaryTreeNode?) {
    if (node == null) return
    dfsPrintInOrder(node.left)
    print("${node.data} ")
    dfsPrintInOrder(node.right)
}
```

### Post-Order (Left -> Right -> Root)
Used for deleting trees or calculating the size of a directory.

```kotlin
fun dfsPrintPostOrder(node: BinaryTreeNode?) {
    if (node == null) return
    dfsPrintPostOrder(node.left)
    dfsPrintPostOrder(node.right)
    print("${node.data} ")
}
```

## 2. Breadth-First Search (BFS)
BFS, also known as **Level-Order Traversal**, visits all nodes at the current depth before moving to the next level. We use a **Queue** to keep track of nodes to visit.



```kotlin
fun bfsReturnOrder(root: BinaryTreeNode): List<List<Int>> {
    val queue = ArrayDeque<BinaryTreeNode>()
    val outputList = mutableListOf<List<Int>>()
    queue.add(root)

    while (queue.isNotEmpty()) {
        val currQueueLength = queue.size
        val levelList = mutableListOf<Int>()
        
        for (i in 0 until currQueueLength) {
            val currNode = queue.removeFirst()
            levelList.add(currNode.data)

            if (currNode.left != null) queue.add(currNode.left!!)
            if (currNode.right != null) queue.add(currNode.right!!)
        }
        outputList.add(levelList)
    }
    return outputList
}
```

### DFS vs BFS: Which one to use?

| Feature | DFS (Recursive) | BFS (Iterative) |
| -- | -- | -- |
| Data Structure | Stack (Call Stack) | Queue |
| Memory | O(Height) | O(Width) |
| Best For | Finding paths, Game Trees | Shortest path, Level grouping


## Summary

DFS is usually easier to implement via recursion, but BFS is essential when you need to process a tree level-by-level. Understanding the timing of when you "visit" the root (Pre, In, or Post) is the key to solving most tree-based interview questions.

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/DSABinaryTrees.kt).
{: .prompt-tip }