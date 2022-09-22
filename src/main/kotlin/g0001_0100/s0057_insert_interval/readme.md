[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 57\. Insert Interval

Medium

You are given an array of non-overlapping intervals `intervals` where <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> represent the start and the end of the <code>i<sup>th</sup></code> interval and `intervals` is sorted in ascending order by <code>start<sub>i</sub></code>. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by <code>start<sub>i</sub></code> and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` _after the insertion_.

**Example 1:**

**Input:** intervals = \[\[1,3],[6,9]], newInterval = [2,5]

**Output:** [[1,5],[6,9]]

**Example 2:**

**Input:** intervals = \[\[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]

**Output:** [[1,2],[3,10],[12,16]]

**Explanation:** Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].

**Constraints:**

*   <code>0 <= intervals.length <= 10<sup>4</sup></code>
*   `intervals[i].length == 2`
*   <code>0 <= start<sub>i</sub> <= end<sub>i</sub> <= 10<sup>5</sup></code>
*   `intervals` is sorted by <code>start<sub>i</sub></code> in **ascending** order.
*   `newInterval.length == 2`
*   <code>0 <= start <= end <= 10<sup>5</sup></code>

## Solution

```kotlin
import java.util.Arrays

class Solution {
    fun insert(intervals: Array<IntArray>, newInterval: IntArray): Array<IntArray> {
        val n = intervals.size
        var l = 0
        var r = n - 1
        while (l < n && newInterval[0] > intervals[l][1]) {
            l++
        }
        while (r >= 0 && newInterval[1] < intervals[r][0]) {
            r--
        }
        val res = Array(l + n - r) { IntArray(2) }
        for (i in 0 until l) {
            res[i] = Arrays.copyOf(intervals[i], intervals[i].size)
        }
        res[l][0] = Math.min(newInterval[0], if (l == n) newInterval[0] else intervals[l][0])
        res[l][1] = Math.max(newInterval[1], if (r == -1) newInterval[1] else intervals[r][1])
        var i = l + 1
        var j = r + 1
        while (j < n) {
            res[i] = intervals[j]
            i++
            j++
        }
        return res
    }
}
```