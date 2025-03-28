[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 435\. Non-overlapping Intervals

Medium

Given an array of intervals `intervals` where <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>, return _the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping_.

**Example 1:**

**Input:** intervals = \[\[1,2],[2,3],[3,4],[1,3]]

**Output:** 1

**Explanation:** [1,3] can be removed and the rest of the intervals are non-overlapping.

**Example 2:**

**Input:** intervals = \[\[1,2],[1,2],[1,2]]

**Output:** 2

**Explanation:** You need to remove two [1,2] to make the rest of the intervals non-overlapping.

**Example 3:**

**Input:** intervals = \[\[1,2],[2,3]]

**Output:** 0

**Explanation:** You don't need to remove any of the intervals since they're already non-overlapping.

**Constraints:**

*   <code>1 <= intervals.length <= 10<sup>5</sup></code>
*   `intervals[i].length == 2`
*   <code>-5 * 10<sup>4</sup> <= start<sub>i</sub> < end<sub>i</sub> <= 5 * 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun eraseOverlapIntervals(intervals: Array<IntArray>): Int {
        intervals.sortWith { a: IntArray, b: IntArray ->
            if (a[0] != b[0]
            ) {
                a[0] - b[0]
            } else {
                a[1] - b[1]
            }
        }
        var erasures = 0
        var end = intervals[0][1]
        for (i in 1 until intervals.size) {
            end = if (intervals[i][0] < end) {
                erasures++
                Math.min(end, intervals[i][1])
            } else {
                intervals[i][1]
            }
        }
        return erasures
    }
}
```