---
title: "LeetCode 876: Finding the Middle Node of a Linked List"
date: 2026-02-14
description: "Solving LeetCode's Middle Node problem in Kotlin, efficiently finding the middle node of a singly linked list without needing to know its length."
categories: [LeetCode, Linked Lists]
tags: [linked-list, middle-node, algorithm, two-pointers]
math: false
---

The **Middle Node** problem on LeetCode ([LeetCode Link](https://leetcode.com/problems/middle-of-linked-list/)) presents a clever challenge: finding the *nth* node from the end of a singly linked list, where 'n' is given as an integer. The twist? You *don’t* know the length of the linked list beforehand! This blog post will explore a Kotlin solution that leverages a two-pointer approach to achieve this efficiently.

## The Problem

Given the head of a singly linked list, return the *nth* node from the end of the list.

**Note:**
*   The length of the linked list is *unknown*.
*   You are given an integer `n` which represents the position of the node from the end of the list.

**Example:**

```kotlin
// Example:  Given a linked list: 1->2->3->4->5, and n = 2, return node 4.
val rootNode = ListNode(1)
val firstNode = ListNode(2)
val secondNode = ListNode(3)
val thirdNode = ListNode(4)
val fourthNode = ListNode(5)

rootNode.next = firstNode
firstNode.next = secondNode
secondNode.next = thirdNode
thirdNode.next = fourthNode

println(LeetCode_876_Middle_Linked_List.middleNode(rootNode)?.`val`) // Output: 4
```

## Approach: Two-Pointer Technique (Fast and Slow)

The most efficient solution to this problem is using the **Two-Pointer Technique**, often referred to as the "Fast and Slow Pointer" approach. This method avoids calculating the length of the linked list, which would add unnecessary complexity and potentially exceed time limits.

1.  **Calculate Length:** We first iterate through the linked list to determine its length (`llsize`).
2.  **Adjust Slow Pointer:** We then divide `llsize` by 2 to find the position of the middle node from the beginning.  We use a second pointer (`currNode`) to move through the list, stepping one node at a time until it reaches the desired position.
3.  **Return Node:** Finally, we return `currNode`, which now points to the middle node.

## The Code (Kotlin)

```kotlin
object LeetCode_876_Middle_Linked_List {

    fun middleNode(head: ListNode?): ListNode? {
        var currNode = head
        var llsize = 0

        while(currNode != null) {
            llsize++
            currNode = currNode.next
        }

        llsize /= 2
        currNode = head

        while(llsize != 0) {
            llsize--
            currNode = currNode?.next
        }

        return currNode
    }
}

fun main() {
    val rootNode = ListNode(1)
    val firstNode = ListNode(2)
    val secondNode = ListNode(3)
    val thirdNode = ListNode(4)
    val fourthNode = ListNode(5)

    rootNode.next = firstNode
    firstNode.next = secondNode
    secondNode.next = thirdNode
    thirdNode.next = fourthNode

    println(LeetCode_876_Middle_Linked_List.middleNode(rootNode)?.`val`)
}
```

## Complexity Analysis

| Metric | Complexity | Explanation |
|---|---|---|
| **Time** | $O(N)$ | We iterate through the linked list twice – once to calculate length and again to find the middle node. |
| **Space** | $O(1)$ | We use a constant amount of extra space (only two pointers). |

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_876_Middle_Linked_List.kt).
{: .prompt-tip }

---