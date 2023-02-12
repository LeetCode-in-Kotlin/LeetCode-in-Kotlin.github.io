[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 641\. Design Circular Deque

Medium

Design your implementation of the circular double-ended queue (deque).

Implement the `MyCircularDeque` class:

*   `MyCircularDeque(int k)` Initializes the deque with a maximum size of `k`.
*   `boolean insertFront()` Adds an item at the front of Deque. Returns `true` if the operation is successful, or `false` otherwise.
*   `boolean insertLast()` Adds an item at the rear of Deque. Returns `true` if the operation is successful, or `false` otherwise.
*   `boolean deleteFront()` Deletes an item from the front of Deque. Returns `true` if the operation is successful, or `false` otherwise.
*   `boolean deleteLast()` Deletes an item from the rear of Deque. Returns `true` if the operation is successful, or `false` otherwise.
*   `int getFront()` Returns the front item from the Deque. Returns `-1` if the deque is empty.
*   `int getRear()` Returns the last item from Deque. Returns `-1` if the deque is empty.
*   `boolean isEmpty()` Returns `true` if the deque is empty, or `false` otherwise.
*   `boolean isFull()` Returns `true` if the deque is full, or `false` otherwise.

**Example 1:**

**Input**

["MyCircularDeque", "insertLast", "insertLast", "insertFront", "insertFront", "getRear", "isFull", "deleteLast", "insertFront", "getFront"]

[[3], [1], [2], [3], [4], [], [], [], [4], []]

**Output:** [null, true, true, true, false, 2, true, true, true, 4]

**Explanation:**

MyCircularDeque myCircularDeque = new MyCircularDeque(3);

myCircularDeque.insertLast(1); // return True

myCircularDeque.insertLast(2); // return True

myCircularDeque.insertFront(3); // return True

myCircularDeque.insertFront(4); // return False, the queue is full.

myCircularDeque.getRear(); // return 2

myCircularDeque.isFull(); // return True

myCircularDeque.deleteLast(); // return True

myCircularDeque.insertFront(4); // return True

myCircularDeque.getFront(); // return 4

**Constraints:**

*   `1 <= k <= 1000`
*   `0 <= value <= 1000`
*   At most `2000` calls will be made to `insertFront`, `insertLast`, `deleteFront`, `deleteLast`, `getFront`, `getRear`, `isEmpty`, `isFull`.

## Solution

```kotlin
class MyCircularDeque(k: Int) {
    private val data: IntArray
    private var front: Int
    private var rear: Int
    private var size: Int

    init {
        data = IntArray(k)
        front = 0
        rear = k - 1
        size = 0
    }

    fun insertFront(value: Int): Boolean {
        if (size == data.size) {
            return false
        }
        data[front] = value
        front = (front + 1) % data.size
        size++
        return true
    }

    fun insertLast(value: Int): Boolean {
        if (size == data.size) {
            return false
        }
        data[rear] = value
        rear = (rear - 1 + data.size) % data.size
        size++
        return true
    }

    fun deleteFront(): Boolean {
        if (size == 0) {
            return false
        }
        front = (front - 1 + data.size) % data.size
        size--
        return true
    }

    fun deleteLast(): Boolean {
        if (size == 0) {
            return false
        }
        rear = (rear + 1) % data.size
        size--
        return true
    }

    fun getFront(): Int {
        return if (size == 0) {
            -1
        } else data[(front - 1 + data.size) % data.size]
    }

    fun getRear(): Int {
        return if (size == 0) {
            -1
        } else data[(rear + 1) % data.size]
    }

    fun isEmpty(): Boolean {
        return size == 0
    }
    fun isFull(): Boolean {
        return size == data.size
    }
}

/*
 * Your MyCircularDeque object will be instantiated and called as such:
 * var obj = MyCircularDeque(k)
 * var param_1 = obj.insertFront(value)
 * var param_2 = obj.insertLast(value)
 * var param_3 = obj.deleteFront()
 * var param_4 = obj.deleteLast()
 * var param_5 = obj.getFront()
 * var param_6 = obj.getRear()
 * var param_7 = obj.isEmpty()
 * var param_8 = obj.isFull()
 */
```