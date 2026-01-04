---
title: "Singly Linked Lists in Kotlin: The Complete Guide"
description: "Singly Linked Lists in Kotlin: The Complete Guide"
date: 2026-01-03
categories: [Algorithms, linked list]
tags: [dsa, kotlin, linked-list, tutorial, pointers]
---

While Arrays are great for indexed access, **Linked Lists** shine when it comes to dynamic memory allocation and efficient insertions/deletions. In a Singly Linked List, each element is a "Node" that contains data and a reference to the next node in the sequence.

In this guide, we will implement a Singly Linked List in Kotlin and cover all essential operations: Insertion, Deletion, and Reversal.

---

## The Foundation: The Node Class

In Kotlin, we can represent a Node using a `data class`.

```kotlin
data class Node(val data: Int, var next: Node? = null)
```

## 1. Insertion Operations
Unlike arrays, we don't need to "shift" elements when inserting into a Linked List. We simply update the pointers.

#### Insert at Head ($O(1)$)
```kotlin
fun insertAtFirst(head: Node?, key: Int): Node {
    val newNode = Node(data = key)
    newNode.next = head
    return newNode
}
```

#### Insert at End ($O(n)$)
We must traverse to the very last node where next is null, then attach our new node.

```kotlin
fun insertAtEnd(head: Node, key: Int): Node {
    val newNode = Node(data = key)
    var currNode: Node? = head
    while (currNode?.next != null) {
        currNode = currNode.next
    }
    currNode?.next = newNode
    return head
}
```
## 2. Deletion Operations

To delete a node, we find the node **before** the one we want to remove and change its `next` pointer to skip the unwanted node.

### Delete at Position

```kotlin
fun deleteNodeAtPosition(head: Node?, position: Int): Node? {
    if (head == null) return head
    if (position == 1) {
        val newHead = head.next
        head.next = null
        return newHead
    }

    var currNode = head
    for (i in 1..< position - 1) {
        if (currNode == null) break
        currNode = currNode.next
    }

    if (currNode?.next == null) return head
    
    currNode.next = currNode.next?.next
    return head
}
```

## 3. The Interview Classic: Reversing a Linked List

Reversing a list in-place requires managing three pointers: `prev`, `current`, and `next`.

```kotlin
fun reverseLinkedList(head: Node?): Node? {
    if (head == null) return head

    var current: Node? = head
    var prev: Node? = null
    var next: Node? = null

    while (current != null) {
        next = current.next    // 1. Save next
        current.next = prev    // 2. Reverse link
        
        prev = current         // 3. Move prev forward
        current = next         // 4. Move current forward
    }
    return prev // New head
}
```

## Summary Table: Time Complexity

| Operation | Complexity | Description |
| --- | --- | --- |
|**Access/Search**|**O(n)**|**Must traverse from head.**| 
|Insert (Head)|O(1)|Immediate pointer update.|
|Insert (End)|$O(n)|Must find the tail first.|
|Delete (Head)|O(1)|Move the head pointer.|

## Source Code
> 🚀 **Code:** All the code for this tutorial is available in my [DSA-Kotlin Repository](https://github.com/purushyb/dsa-kotlin/blob/main/src/DSALinkedList.kt).
{: .prompt-tip }