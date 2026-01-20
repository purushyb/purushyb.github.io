---
title: "LeetCode 133: Clone Graph"
date: 2026-01-20
description: "A step-by-step guide to deep copying a graph using Breadth-First Search (BFS) and HashMaps in Kotlin."
categories: [LeetCode, Graphs]
tags: [leetcode, graphs, bfs, hashmap, kotlin, medium]
math: true
---

Graph problems often require us to traverse nodes while maintaining relationships. **Cloning a Graph** is a classic problem that tests your ability to traverse a structure while creating a deep copy of it, ensuring that the new graph has the exact same structure but consists of entirely new node instances.

In this post, we'll solve LeetCode 133 using **Breadth-First Search (BFS)**.

## The Problem
Given a reference of a node in a **connected undirected graph**, return a **deep copy** (clone) of the graph.

Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors.

```kotlin
class Node(var `val`: Int) {
    var neighbors: ArrayList<Node?> = ArrayList<Node?>()
}
```

## Approach: BFS + HashMap

The core challenge in cloning a graph is handling **cycles** and **shared neighbors**. If node A is connected to B, and B is connected back to A, a naive recursive clone might get stuck in an infinite loop.

To solve this, we use a `HashMap` to keep track of nodes we have already cloned.
- **Key:** The original node's value (or reference).
- **Value:** The newly created clone node.

### Algorithm
1.  **Initialize**: Create a `HashMap` to store `visited` nodes (mapping `original value -> clone`) and a `Queue` for BFS.
2.  **Start**: Clone the starting node, add it to the map, and push the original node into the queue.
3.  **Traverse**: While the queue is not empty:
    *   Dequeue the current node.
    *   Iterate through its `neighbors`.
    *   If a neighbor hasn't been visited (not in the map):
        *   Create a clone of the neighbor.
        *   Add it to the map.
        *   Enqueue the original neighbor.
    *   **Link**: Add the *cloned* neighbor to the *cloned* current node's neighbor list.

### The Code

Here is the complete implementation in Kotlin:

```kotlin
import java.util.LinkedList

object LeetCode_133_clone_graph {

    class Node(var `val`: Int) {
        var neighbors: ArrayList<Node?> = ArrayList<Node?>()

        override fun toString(): String {
            return "Node(${"`val`"})"
        }
    }

    fun cloneGraph(node: Node?): Node? {
        if (node == null) return null

        // Map to keep track of created nodes: Original Node Value -> New Node
        val map = HashMap<Int, Node>()
        val q = LinkedList<Node>()

        // 1. Clone the root
        val rootNode = Node(node.`val`)
        map[node.`val`] = rootNode
        q.offer(node)

        // 2. BFS Traversal
        while (q.isNotEmpty()) {
            val currNode = q.poll()

            currNode.neighbors.forEach { neighbor ->
                // If neighbor is visited/created for the first time
                if (!map.containsKey(neighbor!!.`val`)) {
                    map[neighbor.`val`] = Node(neighbor.`val`)
                    q.offer(neighbor)
                }

                // Link the clone of the current node to the clone of the neighbor
                map[currNode.`val`]?.neighbors?.add(map[neighbor.`val`])
            }
        }
        return map[node.`val`]
    }
}
```

## Complexity Analysis

| Metric | Complexity | Explanation |
|---|---|---|
| **Time** | $O(V + E)$ | We process each vertex ($V$) and each edge ($E$) exactly once during the BFS traversal. |
| **Space** | $O(V)$ | The `HashMap` stores all $V$ vertices, and the `Queue` can grow up to $O(V)$ in the worst case. |

## Testing the Solution
To verify the implementation, we can create a graph using an adjacency list, clone it, and print the structure.

```kotlin
fun main() {
    val graphRoot = LeetCode_133_clone_graph.createGraph()

    println("== Original Graph ==")
    if (graphRoot != null) {
        LeetCode_133_clone_graph.printGraph(graphRoot)
    }

    println("\n== Clone Graph ==")
    val cloneGraph = LeetCode_133_clone_graph.cloneGraph(graphRoot)
    LeetCode_133_clone_graph.printGraph(cloneGraph!!)
}
```

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_133_clone_graph.kt).
{: .prompt-tip }

```