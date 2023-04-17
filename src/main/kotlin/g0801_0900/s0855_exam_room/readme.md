[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 855\. Exam Room

Medium

There is an exam room with `n` seats in a single row labeled from `0` to `n - 1`.

When a student enters the room, they must sit in the seat that maximizes the distance to the closest person. If there are multiple such seats, they sit in the seat with the lowest number. If no one is in the room, then the student sits at seat number `0`.

Design a class that simulates the mentioned exam room.

Implement the `ExamRoom` class:

*   `ExamRoom(int n)` Initializes the object of the exam room with the number of the seats `n`.
*   `int seat()` Returns the label of the seat at which the next student will set.
*   `void leave(int p)` Indicates that the student sitting at seat `p` will leave the room. It is guaranteed that there will be a student sitting at seat `p`.

**Example 1:**

**Input** ["ExamRoom", "seat", "seat", "seat", "seat", "leave", "seat"] [[10], [], [], [], [], [4], []]

**Output:** [null, 0, 9, 4, 2, null, 5]

**Explanation:**

    ExamRoom examRoom = new ExamRoom(10);
    examRoom.seat(); // return 0, no one is in the room, then the student sits at seat number 0. 
    examRoom.seat(); // return 9, the student sits at the last seat number 9. 
    examRoom.seat(); // return 4, the student sits at the last seat number 4. 
    examRoom.seat(); // return 2, the student sits at the last seat number 2. 
    examRoom.leave(4); 
    examRoom.seat(); // return 5, the student sits at the last seat number 5.

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>
*   It is guaranteed that there is a student sitting at seat `p`.
*   At most <code>10<sup>4</sup></code> calls will be made to `seat` and `leave`.

## Solution

```kotlin
class ExamRoom() {
    private class Node(var `val`: Int, map: MutableMap<Int?, Node>) {
        var pre: Node? = null
        var next: Node? = null

        init {
            map[`val`] = this
        }

        fun insert(left: Node?): Int {
            val right = left!!.next
            left.next = this
            right!!.pre = this
            next = right
            pre = left
            return `val`
        }

        fun delete() {
            val left = pre
            val right = next
            left!!.next = right
            right!!.pre = left
        }
    }

    private val map: MutableMap<Int?, Node> = HashMap()
    private val head = Node(-1, map)
    private val tail = Node(-1, map)
    private var n = 0

    init {
        head.next = tail
        tail.pre = head
    }

    constructor(n: Int) : this() {
        this.n = n
    }

    fun seat(): Int {
        val right = n - 1 - tail.pre!!.`val`
        val left = head.next!!.`val`
        var max = 0
        var maxAt = -1
        var maxAtLeft: Node? = null
        var cur = tail.pre
        while (cur !== head && cur!!.pre !== head) {
            val pre = cur!!.pre
            val at = (cur.`val` + pre!!.`val`) / 2
            val distance = at - pre.`val`
            if (distance >= max) {
                maxAtLeft = pre
                max = distance
                maxAt = at
            }
            cur = cur.pre
        }
        if (head.next === tail || left >= max && left >= right) {
            return Node(0, map).insert(head)
        }
        return if (right > max) {
            Node(n - 1, map).insert(tail.pre)
        } else Node(maxAt, map).insert(maxAtLeft)
    }

    fun leave(p: Int) {
        map[p]!!.delete()
        map.remove(p)
    }
}

/*
 * Your ExamRoom object will be instantiated and called as such:
 * var obj = ExamRoom(n)
 * var param_1 = obj.seat()
 * obj.leave(p)
 */
```