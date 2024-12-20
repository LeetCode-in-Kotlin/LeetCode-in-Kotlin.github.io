[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 622\. Design Circular Queue

Medium

Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Implementation the `MyCircularQueue` class:

*   `MyCircularQueue(k)` Initializes the object with the size of the queue to be `k`.
*   `int Front()` Gets the front item from the queue. If the queue is empty, return `-1`.
*   `int Rear()` Gets the last item from the queue. If the queue is empty, return `-1`.
*   `boolean enQueue(int value)` Inserts an element into the circular queue. Return `true` if the operation is successful.
*   `boolean deQueue()` Deletes an element from the circular queue. Return `true` if the operation is successful.
*   `boolean isEmpty()` Checks whether the circular queue is empty or not.
*   `boolean isFull()` Checks whether the circular queue is full or not.

You must solve the problem without using the built-in queue data structure in your programming language.

**Example 1:**

**Input** ["MyCircularQueue", "enQueue", "enQueue", "enQueue", "enQueue", "Rear", "isFull", "deQueue", "enQueue", "Rear"] [[3], [1], [2], [3], [4], [], [], [], [4], []]

**Output:** [null, true, true, true, false, 3, true, true, true, 4]

**Explanation:** MyCircularQueue myCircularQueue = new MyCircularQueue(3); myCircularQueue.enQueue(1); // return True myCircularQueue.enQueue(2); // return True myCircularQueue.enQueue(3); // return True myCircularQueue.enQueue(4); // return False myCircularQueue.Rear(); // return 3 myCircularQueue.isFull(); // return True myCircularQueue.deQueue(); // return True myCircularQueue.enQueue(4); // return True myCircularQueue.Rear(); // return 4

**Constraints:**

*   `1 <= k <= 1000`
*   `0 <= value <= 1000`
*   At most `3000` calls will be made to `enQueue`, `deQueue`, `Front`, `Rear`, `isEmpty`, and `isFull`.

## Solution

```kotlin
class MyCircularQueue(private val maxSize: Int) {
    private val dumyHead = DoubleLinkedNode(0)
    private var size = 0

    init {
        dumyHead.left = dumyHead
        dumyHead.right = dumyHead
    }

    fun enQueue(value: Int): Boolean {
        if (size == maxSize) {
            return false
        }
        val node = DoubleLinkedNode(value)
        val right = dumyHead.right
        dumyHead.right = node
        node.left = dumyHead
        node.right = right
        right!!.left = node
        size++
        return true
    }

    fun deQueue(): Boolean {
        if (size == 0) {
            return false
        }
        val left = dumyHead.left
        dumyHead.left = left!!.left
        dumyHead.left!!.right = dumyHead
        size--
        return true
    }

    fun Rear(): Int {
        return if (size == 0) {
            -1
        } else {
            dumyHead.right!!.`val`
        }
    }

    fun Front(): Int {
        return if (size == 0) {
            -1
        } else {
            dumyHead.left!!.`val`
        }
    }

    fun isEmpty(): Boolean {
        return size == 0
    }

    fun isFull(): Boolean {
        return size == maxSize
    }

    internal class DoubleLinkedNode(val `val`: Int) {
        var left: DoubleLinkedNode? = null
        var right: DoubleLinkedNode? = null
    }
}

/*
 * Your MyCircularQueue object will be instantiated and called as such:
 * var obj = MyCircularQueue(k)
 * var param_1 = obj.enQueue(value)
 * var param_2 = obj.deQueue()
 * var param_3 = obj.Front()
 * var param_4 = obj.Rear()
 * var param_5 = obj.isEmpty()
 * var param_6 = obj.isFull()
 */
```