[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 56\. Merge Intervals

Medium

Given an array of `intervals` where <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

**Example 1:**

**Input:** intervals = \[\[1,3],[2,6],[8,10],[15,18]]

**Output:** [[1,6],[8,10],[15,18]]

**Explanation:** Since intervals [1,3] and [2,6] overlap, merge them into [1,6].

**Example 2:**

**Input:** intervals = \[\[1,4],[4,5]]

**Output:** [[1,5]]

**Explanation:** Intervals [1,4] and [4,5] are considered overlapping.

**Constraints:**

*   <code>1 <= intervals.length <= 10<sup>4</sup></code>
*   `intervals[i].length == 2`
*   <code>0 <= start<sub>i</sub> <= end<sub>i</sub> <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun merge(intervals: Array<IntArray>): Array<IntArray> {
        val result: MutableList<IntArray> = mutableListOf()
        if (intervals.size <= 1) return intervals
        intervals.sortBy { it.first() }
        var currentRange = 1
        var begin = intervals[0][0]
        var end = intervals[0][1]
        while (currentRange < intervals.size) {
            if (intervals[currentRange][0] > end) {
                result.add(intArrayOf(begin, end))
                begin = intervals[currentRange][0]
                end = intervals[currentRange][1]
            } else if (intervals[currentRange][1] > end) {
                end = intervals[currentRange][1]
            }
            currentRange++
        }
        result.add(intArrayOf(begin, end))
        return result.toTypedArray()
    }
}
```