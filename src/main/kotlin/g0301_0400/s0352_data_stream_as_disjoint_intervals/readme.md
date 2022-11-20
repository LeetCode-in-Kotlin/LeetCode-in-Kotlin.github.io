[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 352\. Data Stream as Disjoint Intervals

Hard

Given a data stream input of non-negative integers <code>a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n</sub></code>, summarize the numbers seen so far as a list of disjoint intervals.

Implement the `SummaryRanges` class:

*   `SummaryRanges()` Initializes the object with an empty stream.
*   `void addNum(int val)` Adds the integer `val` to the stream.
*   `int[][] getIntervals()` Returns a summary of the integers in the stream currently as a list of disjoint intervals <code>[start<sub>i</sub>, end<sub>i</sub>]</code>.

**Example 1:**

**Input**

    ["SummaryRanges", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals"]
    [[], [1], [], [3], [], [7], [], [2], [], [6], []]

**Output:** [null, null, [[1, 1]], null, [[1, 1], [3, 3]], null, [[1, 1], [3, 3], [7, 7]], null, [[1, 3], [7, 7]], null, [[1, 3], [6, 7]]]

**Explanation:**

    SummaryRanges summaryRanges = new SummaryRanges();
    summaryRanges.addNum(1); // arr = [1]
    summaryRanges.getIntervals(); // return [[1, 1]]
    summaryRanges.addNum(3); // arr = [1, 3]
    summaryRanges.getIntervals(); // return [[1, 1], [3, 3]]
    summaryRanges.addNum(7); // arr = [1, 3, 7]
    summaryRanges.getIntervals(); // return [[1, 1], [3, 3], [7, 7]]
    summaryRanges.addNum(2); // arr = [1, 2, 3, 7]
    summaryRanges.getIntervals(); // return [[1, 3], [7, 7]]
    summaryRanges.addNum(6); // arr = [1, 2, 3, 6, 7]
    summaryRanges.getIntervals(); // return [[1, 3], [6, 7]]

**Constraints:**

*   <code>0 <= val <= 10<sup>4</sup></code>
*   At most <code>3 * 10<sup>4</sup></code> calls will be made to `addNum` and `getIntervals`.

**Follow up:** What if there are lots of merges and the number of disjoint intervals is small compared to the size of the data stream?

## Solution

```kotlin
class SummaryRanges {
    private class Node(var min: Int) {
        var max: Int

        init {
            max = min
        }
    }

    private val list: MutableList<Node>

    init {
        list = ArrayList()
    }

    fun addNum(`val`: Int) {
        var left = 0
        var right = list.size - 1
        var index = list.size
        while (left <= right) {
            val mid = left + (right - left) / 2
            val node = list[mid]
            if (node.min <= `val` && `val` <= node.max) {
                return
            } else if (`val` < node.min) {
                index = mid
                right = mid - 1
            } else if (`val` > node.max) {
                left = mid + 1
            }
        }
        list.add(index, Node(`val`))
    }

    fun getIntervals(): Array<IntArray> {
        var i = 1
        while (i < list.size) {
            val prev = list[i - 1]
            val curr = list[i]
            if (curr.min == prev.max + 1) {
                prev.max = curr.max
                list.removeAt(i--)
            }
            i++
        }
        val len = list.size
        val res = Array(len) { IntArray(2) }
        for (j in 0 until len) {
            val node = list[j]
            res[j][0] = node.min
            res[j][1] = node.max
        }
        return res
    }
}

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * var obj = SummaryRanges()
 * obj.addNum(value)
 * var param_2 = obj.getIntervals()
 */
```