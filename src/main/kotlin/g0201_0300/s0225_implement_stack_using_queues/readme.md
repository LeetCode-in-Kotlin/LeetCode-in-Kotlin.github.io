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
import java.util.Queue

class MyStack() {
    private val queue1: Queue<Int> = LinkedList()
    private val queue2: Queue<Int> = LinkedList()

    fun push(x: Int) {
        queue1.add(x)
    }

    fun pop(): Int {
        while (queue1.size > 1) {
            queue2.add(queue1.remove())
        }
        val top = queue1.remove()
        queue1.clear()
        queue1.addAll(queue2)
        queue2.clear()
        return top
    }

    fun top(): Int {
        while (queue1.size > 1) {
            queue2.add(queue1.remove())
        }
        val top = queue1.remove()
        queue2.add(top)
        queue1.clear()
        queue1.addAll(queue2)
        queue2.clear()
        return top
    }

    fun empty(): Boolean {
        return queue1.isEmpty()
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