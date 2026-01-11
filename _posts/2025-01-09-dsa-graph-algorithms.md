---
title: "Graph Theory: BFS, DFS, and Dijkstra's Algorithm"
date: 2026-01-09
description: "A comprehensive guide to traversing graphs and finding the shortest path between nodes using Kotlin."
categories: [Algorithms, graphs]
tags: [dsa, kotlin, graphs, bfs, dfs, dijkstra, cycle-detection]
math: true
---

Graphs are one of the most versatile data structures in computer science, used to model everything from social networks to GPS navigation. In this post, we explore how to traverse a graph and how to find the most efficient path between two points.

---

## 1. Graph Traversal: BFS vs DFS

To visit nodes in a graph, we primarily use two strategies: **Depth-First Search** and **Breadth-First Search**.



### Depth-First Search (DFS)
DFS explores as far as possible along each branch before backtracking. It is naturally implemented using **recursion** (which uses the call stack).

```kotlin
fun dfs(adjList: Array<Array<Int>>): List<Int> {
    val result = mutableListOf<Int>()
    dfsRecursion(adjList, mutableMapOf<Int, Boolean>(), result, 0)
    return result
}

fun dfsRecursion(
    adjList: Array<Array<Int>>,
    visitedNodes: MutableMap<Int, Boolean>,
    output: MutableList<Int>,
    currentNode: Int
) {
    visitedNodes[currentNode] = true
    output.add(currentNode)

    for (neighbor in adjList[currentNode]) {
        if (!visitedNodes.getOrDefault(neighbor, false)) {
            dfsRecursion(adjList, visitedNodes, output, neighbor)
        }
    }
}
```

### Breadth-First Search (BFS)

BFS explores neighbor nodes first, before moving to the next level neighbors. It uses a Queue to keep track of the frontier.

```kotlin
fun bfs(adj: Array<Array<Int>>): List<Int> {
    val output = mutableListOf<Int>()
    val queue = LinkedList<Int>()
    val visited = BooleanArray(adj.size) { false }

    visited[0] = true
    queue.offer(0)

    while (queue.isNotEmpty()) {
        val current = queue.poll()
        output.add(current)

        for (neighbor in adj[current]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true
                queue.offer(neighbor)
            }
        }
    }
    return output
}
```

## 2. Cycle Detection in Directed Graphs

Detecting a cycle is critical in many applications, like checking for deadlocks in an OS or resolving dependencies in a build system. We use DFS with a Recursion Stack to track nodes in the current path.

```kotlin
fun isCycleExists(adj: Array<Array<Int>>): Boolean {
    val visitedArray = Array(adj.size) { false }
    val recurArray = Array(adj.size) { false }

    for (i in 0 until adj.size) {
        if (!visitedArray[i] && dfsCycleDetection(adj, visitedArray, recurArray, i)) {
            return true
        }
    }
    return false
}

fun dfsCycleDetection(
    adj: Array<Array<Int>>,
    visited: Array<Boolean>,
    recurStack: Array<Boolean>,
    currElement: Int
): Boolean {
    if (recurStack[currElement]) return true
    if (visited[currElement]) return false

    visited[currElement] = true
    recurStack[currElement] = true

    for (neighbor in adj[currElement]) {
        if (dfsCycleDetection(adj, visited, recurStack, neighbor)) return true
    }

    // Backtrack: remove from current path
    recurStack[currElement] = false
    return false
}
```

## 3. Dijkstra's Shortest Path Algorithm

Dijkstra's algorithm finds the shortest path from a starting node to all other nodes in a weighted graph. It uses a Priority Queue to always expand the node with the current smallest known distance.

### The Implementation

This implementation uses a `PriorityQueue` of `Pair(Node, Distance)` to efficiently pick the next node to process.

```kotlin
fun dijkstraShortestPath(adj: Array<Array<Array<Int>>>, startNode: Int): Array<Int> {
    val distances = Array<Int>(adj.size) { Int.MAX_VALUE }
    val pq = PriorityQueue<Pair<Int, Int>> { a, b -> a.second.compareTo(b.second) }

    distances[startNode] = 0
    pq.offer(Pair(startNode, 0))

    while (pq.isNotEmpty()) {
        val (currentNode, currentDist) = pq.poll()

        // Optimization: skip if we've found a better path already
        if (currentDist > distances[currentNode]) continue

        for (neighborData in adj[currentNode]) {
            val neighbor = neighborData[0]
            val weight = neighborData[1]
            val newDist = currentDist + weight

            if (newDist < distances[neighbor]) {
                distances[neighbor] = newDist
                pq.offer(Pair(neighbor, newDist))
            }
        }
    }
    return distances
}
```

## Complexity Summary

| Algorithm | Algorithm | Algorithm | Best For |
| -- | -- | -- | -- |
| DFS | Stack (Recursion) | _O(V + E)_ | Topology, Pathfinding |
| DFS | Queue | _O(V + E)_ | Shortest path (Unweighted) |
| Dijkstra | Priority Queue | _O(E log V)_ | Shortest path (Weighted) |

Note: _V_ is the number of vertices, and $E$ is the number of edges.

## Summary

- Use DFS when you want to explore every node or detect cycles.
- Use BFS for finding the shortest path in unweighted graphs (like a social network degree of separation).
- Use Dijkstra for weighted graphs (like Google Maps) where the "cost" of edges varies.

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/DSAGraphs.kt).
{: .prompt-tip }

