---
title: "LeetCode 876: Middle Node in a Linked List"
date: 2026-01-21
description: "Solving LeetCode's Middle Node problem in Kotlin, using a clever approach to find the middle node of a singly linked list without knowing its length."
categories: [LeetCode, Middle Of Linked Lists]
tags: [linked-list, middle-node, algorithm, two-pointers]
math: false
---

The **Middle Node** problem on LeetCode ([LeetCode Link](https://leetcode.com/problems/middle-of-linked-list/)) challenges us to find the *nth* node from the end of a singly linked list, where 'n' is given as an integer.  The key to solving this problem efficiently lies in avoiding the need to calculate the length of the linked list beforehand.

In this post, we’ll explore a Kotlin solution that utilizes a two-pointer approach to find the middle node of a linked list in linear time, without needing to know its length.

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

The most efficient way to solve this problem is using the two-pointer technique, often referred to as the "fast and slow pointer" approach.

1.  **Initialize Pointers:** We initialize two pointers, `fastPointer` and `slowPointer`, both starting at the head of the linked list.
2.  **Move Pointers:** Move `fastPointer` two steps at a time, while `slowPointer` moves one step at a time.
3.  **Cycle Detection:** After traversing the linked list for a certain number of steps, `fastPointer` will eventually catch up with `slowPointer`.  This indicates that the linked list has an odd number of nodes. If we want to find the middle node, we can simply move `slowPointer` one step back and continue iterating.
4.  **Return Node:** When the `fastPointer` reaches the end of the linked list, return the node that `slowPointer` is pointing to.

## The Code

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
| **Time** | $O(N)$ | We iterate through the linked list only once. |
| **Space** | $O(1)$ | We use a constant amount of extra space. |

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/LeetCode_876_Middle_Linked_List.kt).
{: .prompt-tip }

---