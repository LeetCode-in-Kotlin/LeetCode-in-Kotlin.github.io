[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 757\. Set Intersection Size At Least Two

Hard

You are given a 2D integer array `intervals` where <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> represents all the integers from <code>start<sub>i</sub></code> to <code>end<sub>i</sub></code> inclusively.

A **containing set** is an array `nums` where each interval from `intervals` has **at least two** integers in `nums`.

*   For example, if `intervals = \[\[1,3], [3,7], [8,9]]`, then `[1,2,4,7,8,9]` and `[2,3,4,8,9]` are **containing sets**.

Return _the minimum possible size of a containing set_.

**Example 1:**

**Input:** intervals = \[\[1,3],[3,7],[8,9]]

**Output:** 5

**Explanation:** let nums = [2, 3, 4, 8, 9]. It can be shown that there cannot be any containing array of size 4.

**Example 2:**

**Input:** intervals = \[\[1,3],[1,4],[2,5],[3,5]]

**Output:** 3

**Explanation:** let nums = [2, 3, 4]. It can be shown that there cannot be any containing array of size 2.

**Example 3:**

**Input:** intervals = \[\[1,2],[2,3],[2,4],[4,5]]

**Output:** 5

**Explanation:** let nums = [1, 2, 3, 4, 5]. It can be shown that there cannot be any containing array of size 4.

**Constraints:**

*   `1 <= intervals.length <= 3000`
*   `intervals[i].length == 2`
*   <code>0 <= start<sub>i</sub> < end<sub>i</sub> <= 10<sup>8</sup></code>

## Solution

```kotlin
class Solution {
    fun intersectionSizeTwo(intervals: Array<IntArray>): Int {
        intervals.sortWith { a, b ->
            if (a[1] == b[1]) {
                b[0] - a[0]
            } else a[1] - b[1]
        }
        val list: MutableList<Int> = ArrayList()
        list.add(intervals[0][1] - 1)
        list.add(intervals[0][1])
        for (i in 1 until intervals.size) {
            val lastOne = list[list.size - 1]
            val lastTwo = list[list.size - 2]
            val interval = intervals[i]
            val start = interval[0]
            val end = interval[1]
            if (lastOne >= start && lastTwo >= start) {
                continue
            } else if (lastOne >= start) {
                list.add(end)
            } else {
                list.add(end - 1)
                list.add(end)
            }
        }
        return list.size
    }
}
```