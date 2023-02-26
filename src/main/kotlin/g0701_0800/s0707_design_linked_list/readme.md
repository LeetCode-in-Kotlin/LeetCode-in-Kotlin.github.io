[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 707\. Design Linked List

Medium

Design your implementation of the linked list. You can choose to use a singly or doubly linked list.  
A node in a singly linked list should have two attributes: `val` and `next`. `val` is the value of the current node, and `next` is a pointer/reference to the next node.  
If you want to use the doubly linked list, you will need one more attribute `prev` to indicate the previous node in the linked list. Assume all nodes in the linked list are **0-indexed**.

Implement the `MyLinkedList` class:

*   `MyLinkedList()` Initializes the `MyLinkedList` object.
*   `int get(int index)` Get the value of the <code>index<sup>th</sup></code> node in the linked list. If the index is invalid, return `-1`.
*   `void addAtHead(int val)` Add a node of value `val` before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
*   `void addAtTail(int val)` Append a node of value `val` as the last element of the linked list.
*   `void addAtIndex(int index, int val)` Add a node of value `val` before the <code>index<sup>th</sup></code> node in the linked list. If `index` equals the length of the linked list, the node will be appended to the end of the linked list. If `index` is greater than the length, the node **will not be inserted**.
*   `void deleteAtIndex(int index)` Delete the <code>index<sup>th</sup></code> node in the linked list, if the index is valid.

**Example 1:**

**Input**

["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]

[[], [1], [3], [1, 2], [1], [1], [1]]

**Output:** [null, null, null, null, 2, null, 3]

**Explanation:**

    MyLinkedList myLinkedList = new MyLinkedList(); 
    myLinkedList.addAtHead(1); 
    myLinkedList.addAtTail(3); 
    myLinkedList.addAtIndex(1, 2); // linked list becomes 1->2->3 
    myLinkedList.get(1); // return 2 
    myLinkedList.deleteAtIndex(1); // now the linked list is 1->3 
    myLinkedList.get(1); // return 3

**Constraints:**

*   `0 <= index, val <= 1000`
*   Please do not use the built-in LinkedList library.
*   At most `2000` calls will be made to `get`, `addAtHead`, `addAtTail`, `addAtIndex` and `deleteAtIndex`.

## Solution

```kotlin
class MyLinkedList {
    private class Node(var `val`: Int) {
        var next: Node? = null
    }

    private var head: Node?
    private var tail: Node? = null
    private var size: Int

    init {
        head = tail
        size = 0
    }

    operator fun get(index: Int): Int {
        if (index >= size) {
            return -1
        }
        if (index == 0) {
            return head!!.`val`
        }
        var i = 0
        var ptr = head
        while (i++ < index - 1) {
            ptr = ptr!!.next
        }
        return ptr!!.next!!.`val`
    }

    fun addAtHead(`val`: Int) {
        val node = Node(`val`)
        if (head == null) {
            tail = node
            head = tail
            size++
            return
        }
        node.next = head
        head = node
        size++
    }

    fun addAtTail(`val`: Int) {
        if (head == null) {
            addAtHead(`val`)
            return
        }
        val node = Node(`val`)
        tail!!.next = node
        tail = node
        size++
    }

    fun addAtIndex(index: Int, `val`: Int) {
        if (index > size) {
            return
        }
        if (index == 0) {
            addAtHead(`val`)
            return
        }
        if (index == size) {
            addAtTail(`val`)
            return
        }
        var i = 0
        val node = Node(`val`)
        var ptr = head
        while (i++ < index - 1) {
            ptr = ptr!!.next
        }
        node.next = ptr!!.next
        ptr.next = node
        size++
    }

    fun deleteAtIndex(index: Int) {
        if (index >= size) {
            return
        }
        if (index == 0) {
            head = head!!.next
            size--
            return
        }
        var i = 0
        var ptr = head
        while (i++ < index - 1) {
            ptr = ptr!!.next
        }
        ptr!!.next = ptr.next!!.next
        if (index == size - 1) {
            tail = ptr
        }
        size--
    }
}

/*
 * Your MyLinkedList object will be instantiated and called as such:
 * var obj = MyLinkedList()
 * var param_1 = obj.get(index)
 * obj.addAtHead(`val`)
 * obj.addAtTail(`val`)
 * obj.addAtIndex(index,`val`)
 * obj.deleteAtIndex(index)
 */
```