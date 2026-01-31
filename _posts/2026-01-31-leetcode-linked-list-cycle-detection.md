---
title: "LeetCode 141: Linked List Cycle"
date: 2026-01-21
description: "Solving LeetCode's Linked List Cycle problem in Kotlin, using the fast and slow pointer approach to efficiently detect cycles."
categories: [LeetCode, Linked List Cycle Detection]
tags: [linked lists, cycle-detection, two-pointers, algorithm]
math: false
---

The **Linked List Cycle** problem on LeetCode ([LeetCode Link](https://leetcode.com/problems/linked-list-cycle/)) presents a classic algorithm challenge that can be solved elegantly using the "fast and slow pointer" (also known as Floyd's cycle-finding algorithm) technique.  This method allows us to efficiently determine if a linked list contains a cycle without needing to know the length of the list.

In this post, we’ll explore a Kotlin solution that utilizes the fast and slow pointer approach to detect cycles in linked lists.

## The Problem
Given a linked list `head` representing *a* cycle, determine if there is a cycle in the linked list.

**Note:**
*   You may not use the values or structure of the linked list other than iterating through it.
*   You may assume that the linked list is not `null`.

**Example:**
```kotlin
// Example:  Cycle exists. 1 -> 2 -> 3 -> 4 -> 5 and 5 points back to 1.
val head = ListNode(3)
head.next = ListNode(2)
head.next?.next = ListNode(0)
head.next?.next?.next = ListNode(4)
head.next?.next?.next?.next = ListNode(1) // Cycle back to head

println(LeetCode_141_Linked_List_Cycle.hasCycle(head)) // Output: true
```

## Approach: Fast and Slow Pointers (Floyd's Algorithm)

The key to solving this problem efficiently is the fast and slow pointer approach. This algorithm leverages the fact that if a cycle exists, the two pointers will eventually meet within the cycle.

1.  **Initialize Pointers:** We initialize two pointers, `slowPointer` and `fastPointer`, both starting at the head of the linked list.
2.  **Move Pointers:** Move `fastPointer` two steps at a time, while `slowPointer` moves one step at a time.
3.  **Cycle Detection:** If either `fastPointer` or `slowPointer` becomes `null`, it means there is no cycle, so return `false`.
4.  **Meeting Point:** If `fastPointer` and `slowPointer` eventually meet at some point, it indicates the presence of a cycle. Return `true`.

## The Code

```kotlin
object LeetCode_141_Linked_List_Cycle {

    fun hasCycle(head: ListNode?): Boolean {
        if (head == null) return false

        var slowPointer = head
        var fastPointer = head.next

        while (slowPointer != null && fastPointer != null && fastPointer.next != null) {

            if (fastPointer == slowPointer) return true

            fastPointer = fastPointer.next?.next
            slowPointer = slowPointer.next

        }

        return false
    }
}

fun main() {
    val rootNode = ListNode(3)
    val firstNode = ListNode(2)
    val secondNode = ListNode(0)
    val thirdNode = ListNode(-4)
    rootNode.next = firstNode
    firstNode.next = secondNode
    secondNode.next = thirdNode
    thirdNode.next = firstNode

    println(LeetCode_141_Linked_List_Cycle.hasCycle(rootNode))
}
```

## Complexity Analysis

| Metric | Complexity | Explanation |
|---|---|---|
| **Time** | $O(N)$ | We iterate through the linked list at most twice (once for each pointer).  Where N is the number of nodes in the linked list. |
| **Space** | $O(1)$ | We use a constant amount of extra space. |

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_141_Linked_List_Cycle.kt).
{: .prompt-tip }

---