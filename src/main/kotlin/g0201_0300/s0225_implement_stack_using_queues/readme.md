[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 225\. Implement Stack using Queues

Easy

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:

*   `void push(int x)` Pushes element x to the top of the stack.
*   `int pop()` Removes the element on the top of the stack and returns it.
*   `int top()` Returns the element on the top of the stack.
*   `boolean empty()` Returns `true` if the stack is empty, `false` otherwise.

**Notes:**

*   You must use **only** standard operations of a queue, which means that only `push to back`, `peek/pop from front`, `size` and `is empty` operations are valid.
*   Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you use only a queue's standard operations.

**Example 1:**

**Input** ["MyStack", "push", "push", "top", "pop", "empty"] [[], [1], [2], [], [], []]

**Output:** [null, null, null, 2, 2, false]

**Explanation:**

    MyStack myStack = new MyStack();
    myStack.push(1);
    myStack.push(2);
    myStack.top(); // return 2
    myStack.pop(); // return 2
    myStack.empty(); // return False 

**Constraints:**

*   `1 <= x <= 9`
*   At most `100` calls will be made to `push`, `pop`, `top`, and `empty`.
*   All the calls to `pop` and `top` are valid.

**Follow-up:** Can you implement the stack using only one queue?

## Solution

```kotlin
import java.util.LinkedList

class MyStack {
    private var queuePair = Pair(LinkedList<Int>(), LinkedList<Int>())
    private var top: Int? = null

    fun push(x: Int) {
        queuePair.first.addLast(x)
        top = x
    }

    fun pop(): Int {
        if (isQueuesEmpty()) {
            throw Exception()
        }
        val queuePair = selectSourceAndDestinationQueues(queuePair)
        var value = 0
        repeat(queuePair.first.size) {
            when (queuePair.first.size) {
                2 -> {
                    top = queuePair.first.removeFirst()
                    queuePair.second.addLast(top)
                }
                1 -> {
                    value = queuePair.first.removeFirst()
                }
                else -> {
                    queuePair.second.addLast(queuePair.first.removeFirst())
                }
            }
        }
        return value
    }

    fun top(): Int {
        if (isQueuesEmpty()) {
            throw Exception()
        }
        return top!!
    }

    fun empty(): Boolean {
        return isQueuesEmpty()
    }

    private fun isQueuesEmpty(): Boolean {
        if (queuePair.first.isEmpty() && queuePair.second.isEmpty()) {
            return true
        }
        return false
    }

    private fun selectSourceAndDestinationQueues(queuePair: Pair<LinkedList<Int>, LinkedList<Int>>):
        Pair<LinkedList<Int>, LinkedList<Int>> {
        return if (queuePair.first.isNotEmpty()) {
            Pair(queuePair.first, queuePair.second)
        } else {
            Pair(queuePair.second, queuePair.first)
        }
    }
}

/*
 * Your MyStack object will be instantiated and called as such:
 * var obj = MyStack()
 * obj.push(x)
 * var param_2 = obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.empty()
 */
```