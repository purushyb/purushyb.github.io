---
title: "Disjoint Set Union (DSU) in Kotlin: Mastering Union-Find"
date: 2026-01-11
description: "A guide to implementing the Disjoint Set Union (DSU) data structure with Path Compression and Union by Rank optimizations."
categories: [Data Structures]
tags: [dsa, kotlin, dsu, union-find, graph, algorithms]
math: true
---

The **Disjoint Set Union (DSU)** (or Union-Find) is a data structure that tracks a set of elements partitioned into a number of disjoint (non-overlapping) subsets. It is incredibly efficient at two operations:
1.  **Find:** Determining which subset a particular element belongs to.
2.  **Union:** Joining two subsets into a single subset.

This structure is the backbone of **Kruskal’s Algorithm** for finding Minimum Spanning Trees and is widely used in network connectivity problems.

---

## The Optimizations

A naive implementation of DSU can result in a specific case resembling a linked list, leading to $O(N)$ time complexity. To make it nearly constant time ($O(\alpha(N))$), we apply two optimizations:

### 1. Path Compression
When we call `find` on a node, we traverse up to the root. With path compression, we make the found root the direct parent of the visited node. This "flattens" the tree structure for future lookups.



```kotlin
fun findWithPathCompression(node: Int, parents: Array<Int>): Int {
    if (parents[node] == node) return node

    // Recursively find the root and update the current node's parent
    parents[node] = findWithPathCompression(parents[node], parents)

    return parents[node]
}
```

## 2. Union by Rank

When merging two sets, we should always attach the shorter tree to the taller tree. This keeps the overall tree height minimal. We use a `rank` array to track the approximate height of each tree.

```kotlin
fun unionByRank(node1: Int, node2: Int, parents: Array<Int>, rank: Array<Int>) {
    val parent1 = findWithPathCompression(node1, parents)
    val parent2 = findWithPathCompression(node2, parents)

    if (parent1 == parent2) return // Already in the same set

    // Attach smaller rank tree under higher rank tree
    if (rank[node1] < rank[node2]) {
        parents[node1] = parent2
    } else if (rank[node1] > rank[node2]) {
        parents[parent2] = parent1
    } else {
        // If ranks are equal, pick one as parent and increment its rank
        parents[node2] = node1
        rank[node1] += 1
    }
}
```

### Putting It Together

Here is how we initialize and use the structure. In the beginning, every node is its own parent (its own set).

```kotlin
fun main() {
    val numberOfElements = 5
    // Initially, rank is 0 for everyone
    val rank = Array<Int>(numberOfElements) { 0 }
    // Initially, every node is its own parent
    val parents = Array<Int>(numberOfElements) { it }

    // Merging sets
    unionByRank(0, 1, parents, rank) // {0, 1}
    unionByRank(2, 3, parents, rank) // {2, 3}
    unionByRank(0, 4, parents, rank) // {0, 1, 4}

    // Checking relationships
    for(i in 0 until numberOfElements) {
        println("$i parent is ${findWithPathCompression(i, parents)}")
    }
}
```

### Complexity Analysis

| Operation | Naive Implementation | With Optimizations |
| --- | --- | --- |
|**Find**|_O(N)_|$O(\alpha(N))$|
|**Union**|_O(N)_|$O(\alpha(N))$|

Note: $\alpha(N)$ is the Inverse Ackermann function, which grows so slowly that for all practical values of $N$ (even up to the number of atoms in the universe), it is $\le 4$. Thus, DSU operations are practically constant time.

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/DSAUnionFindDisjointSets.kt).
{: .prompt-tip }
