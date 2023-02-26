[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 715\. Range Module

Hard

A Range Module is a module that tracks ranges of numbers. Design a data structure to track the ranges represented as **half-open intervals** and query about them.

A **half-open interval** `[left, right)` denotes all the real numbers `x` where `left <= x < right`.

Implement the `RangeModule` class:

*   `RangeModule()` Initializes the object of the data structure.
*   `void addRange(int left, int right)` Adds the **half-open interval** `[left, right)`, tracking every real number in that interval. Adding an interval that partially overlaps with currently tracked numbers should add any numbers in the interval `[left, right)` that are not already tracked.
*   `boolean queryRange(int left, int right)` Returns `true` if every real number in the interval `[left, right)` is currently being tracked, and `false` otherwise.
*   `void removeRange(int left, int right)` Stops tracking every real number currently being tracked in the **half-open interval** `[left, right)`.

**Example 1:**

**Input**

["RangeModule", "addRange", "removeRange", "queryRange", "queryRange", "queryRange"]

[[], [10, 20], [14, 16], [10, 14], [13, 15], [16, 17]]

**Output:** [null, null, null, true, false, true]

**Explanation:**

    RangeModule rangeModule = new RangeModule();
    rangeModule.addRange(10, 20); 
    rangeModule.removeRange(14, 16); 
    rangeModule.queryRange(10, 14); // return True,(Every number in [10, 14) is being tracked) 
    rangeModule.queryRange(13, 15); // return False,(Numbers like 14, 14.03, 14.17 in [13, 15) are not being tracked) 
    rangeModule.queryRange(16, 17); // return True, (The number 16 in [16, 17) is still being tracked, despite the remove operation)

**Constraints:**

*   <code>1 <= left < right <= 10<sup>9</sup></code>
*   At most <code>10<sup>4</sup></code> calls will be made to `addRange`, `queryRange`, and `removeRange`.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class RangeModule {
    private val head: Interval = Interval(0, 0)

    fun addRange(left: Int, right: Int) {
        var left = left
        var right = right
        var pos: Interval? = head
        while (pos!!.next != null && pos.next!!.end < left) {
            pos = pos.next
        }
        while (pos.next != null && pos.next!!.start <= right) {
            left = pos.next!!.start.coerceAtMost(left)
            right = pos.next!!.end.coerceAtLeast(right)
            removeNext(pos)
        }
        insert(pos, Interval(left, right))
    }

    fun queryRange(left: Int, right: Int): Boolean {
        var pos: Interval? = head
        while (pos != null) {
            if (left >= pos.start && right <= pos.end) {
                return true
            }
            pos = pos.next
        }
        return false
    }

    fun removeRange(left: Int, right: Int) {
        var pos: Interval? = head
        while (pos!!.next != null && pos.next!!.end <= left) {
            pos = pos.next
        }
        var prev = pos
        var curr = pos.next
        while (curr != null && curr.start < right) {
            if (curr.start < left) {
                insert(prev, Interval(curr.start, left))
                curr.start = left
                prev = prev!!.next
                curr = prev!!.next
                continue
            }
            if (right >= curr.end) {
                removeNext(prev)
                curr = prev!!.next
            } else {
                curr.start = right
                curr = curr.next
            }
        }
    }

    private fun insert(curr: Interval?, next: Interval) {
        next.next = curr!!.next
        curr.next = next
    }

    private fun removeNext(curr: Interval?) {
        val del = curr!!.next
        if (del != null) {
            curr.next = del.next
            del.next = null
        }
    }

    internal class Interval(var start: Int, var end: Int) {
        var next: Interval? = null
    }
}

/*
 * Your RangeModule object will be instantiated and called as such:
 * var obj = RangeModule()
 * obj.addRange(left,right)
 * var param_2 = obj.queryRange(left,right)
 * obj.removeRange(left,right)
 */
```