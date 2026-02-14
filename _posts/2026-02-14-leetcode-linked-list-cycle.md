---
title: "LeetCode 141: Detecting Linked List Cycles"
date: 2026-02-14
description: "Solving LeetCode's Linked List Cycle problem in Kotlin, utilizing the fast and slow pointer (Floyd's) algorithm for efficient cycle detection."
categories: [LeetCode, Linked Lists]
tags: [linked-list, cycle-detection, two-pointers, algorithm]
math: false
---

The **Linked List Cycle** problem on LeetCode ([LeetCode Link](https://leetcode.com/problems/linked-list-cycle/)) is a classic algorithm challenge that tests your understanding of linked list manipulation and cycle detection. The problem requires you to determine if a given singly linked list contains a cycle (a loop).  This blog post will walk through the solution in Kotlin, explaining the underlying logic and complexity.

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

## Approach: Fast and Slow Pointers (Floyd's Cycle Detection Algorithm)

The most efficient way to solve this problem is by employing the **Fast and Slow Pointer** (also known as Floyd's Cycle Detection Algorithm). This technique leverages the fact that if a cycle exists in a linked list, the fast and slow pointers will eventually meet within the cycle.

1.  **Initialization:** We initialize two pointers, `slowPointer` and `fastPointer`, both starting at the head of the linked list.
2.  **Iteration:** We iterate through the linked list, moving `fastPointer` two steps at a time and `slowPointer` one step at a time.
3.  **Cycle Detection:** If, at any point, `fastPointer` and `slowPointer` become equal, it signifies the presence of a cycle. We can immediately return `true`.
4.  **No Cycle:** If either pointer reaches the end of the linked list (becomes `null`) without meeting, it means there is no cycle and we return `false`.

## The Code (Kotlin)

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
| **Time** | $O(N)$ | We iterate through the linked list at most once.  Where N is the number of nodes in the linked list. |
| **Space** | $O(1)$ | We use a constant amount of extra space (only two pointers). |

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_141_Linked_List_Cycle.kt).
{: .prompt-tip }

---