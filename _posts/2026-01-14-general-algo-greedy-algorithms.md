---
title: "Kruskal's Algorithm: Finding Minimum Spanning Trees in Kotlin"
date: 2026-01-14
description: "Learn how to find the Minimum Spanning Tree (MST) of a graph using Kruskal's Algorithm and the Disjoint Set Union (DSU) data structure."
categories: [General Algorithms, Greedy Algorithms]
tags: [dsa, kotlin, graph, mst, kruskal, greedy-algorithms]
math: true
---

In a weighted graph, a **Minimum Spanning Tree (MST)** is a subset of edges that connects all vertices together, without any cycles and with the minimum possible total edge weight.

Think of it like laying down cables to connect 5 different cities: you want to connect everyone, but you want to buy the least amount of cable possible. **Kruskal's Algorithm** is a greedy strategy to solve this efficiently.

---

## The Logic: Greedy Approach

Kruskal's algorithm is simple and intuitive:
1.  **Sort all edges** from smallest weight to largest weight.
2.  Iterate through the sorted edges.
3.  For each edge, check: **Does this edge connect two nodes that are already connected?**
4.  If **No**: Add the edge to our MST.
5.  If **Yes**: Discard the edge (because adding it would create a cycle).



To efficiently check if two nodes are already connected (Step 3), we use the **Disjoint Set Union (DSU)** data structure we built in the previous post.

---

## The Implementation

We represent the graph as a list of edges, where each edge is `[node1, node2, weight]`.

### Step 1: Sorting and Iterating
We sort the edges based on weight. Then, we use the `find` operation to check if the two nodes of the current edge share the same parent. If they have different parents, we `union` them and add the weight to our total cost.

```kotlin
fun kruskalsMinimumSpanningTree(edges: Array<Array<Int>>, noOfVertices: Int): Int {
    // 1. Initialize DSU arrays
    val parents = Array<Int>(noOfVertices) { it }
    val ranks = Array<Int>(noOfVertices) { 0 }

    // 2. Sort edges by weight (Ascending)
    edges.sortWith(Comparator<Array<Int>> { o1, o2 -> o1[2].compareTo(o2[2]) })

    var totalCost = 0

    // 3. Greedy Iteration
    for (i in edges) {
        val u = i[0]
        val v = i[1]
        val weight = i[2]

        // Check for Cycle: If parents are different, no cycle exists
        if (find(parents, u) != find(parents, v)) {
            totalCost += weight
            union(parents, ranks, u, v)
            println("Edge included: $u -> $v")
        }
    }

    return totalCost
}
```

## Step 2: DSU Helper Functions

hese are the standard Union-Find operations. `find` locates the representative of a set, and `union` merges two sets based on rank.

```kotlin
fun find(parentsArray: Array<Int>, currentElement: Int): Int {
    if (parentsArray[currentElement] == currentElement) return currentElement
    return find(parentsArray, parentsArray[currentElement])
}

fun union(parentsArray: Array<Int>, ranks: Array<Int>, element1: Int, element2: Int) {
    val parent1 = find(parentsArray, element1)
    val parent2 = find(parentsArray, element2)

    if (parent1 == parent2) return

    if (ranks[parent1] > ranks[parent2]) {
        parentsArray[parent2] = parent1
    } else if (ranks[parent1] < ranks[parent2]) {
        parentsArray[parent1] = parent2
    } else {
        ranks[parent1] += 1
        parentsArray[parent1] = parent2
    }
}
```

### Example Run

Consider a graph with 4 vertices and these edges:

- `0-1` (weight 10)
- `0-2` (weight 6)
- `0-3` (weight 5)
- `1-3` (weight 15)
- `2-3` (weight 4)

Execution:

1. Sort: `[2,3,4]`, `[0,3,5]`, `[0,2,6]`, `[0,1,10]`, `[1,3,15]`
2. Pick `2-3` (weight 4): Connect 2 and 3. Cost = 4.
3. Pick `0-3` (weight 5): Connect 0 and 3. Cost = 4+5 = 9.
4. Pick `0-2` (weight 6): 0 and 2 are already connected (via 3). Skip.
5. Pick `0-1` (weight 10): Connect 0 and 1. Cost = 9+10 = 19.
6. Result: Total MST Cost = 19.

## Complexity Analysis

| Step | Complexity |
| --- | --- |
|**Sorting Edges**|$O(E \log E)$|
|**DSU Operations**|$O(E \times \alpha(V))$ (Nearly constant)|
|**Total Time**|$O(N)$| $O(E \log E)$ |

_Where $E$ is the number of edges and $V$ is the number of vertices._

## Summary

Kruskal's Algorithm is elegant because it makes the "greedy" choice (always picking the cheapest available edge) and proves that this choice leads to the globally optimal solution. It is the perfect showcase for the power of the Disjoint Set Union data structure.

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/GeneralTechniquesGreedyAlgorithms.kt).
{: .prompt-tip }