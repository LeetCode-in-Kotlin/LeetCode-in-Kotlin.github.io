[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 436\. Find Right Interval

Medium

You are given an array of `intervals`, where <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> and each <code>start<sub>i</sub></code> is **unique**.

The **right interval** for an interval `i` is an interval `j` such that <code>start<sub>j</sub> >= end<sub>i</sub></code> and <code>start<sub>j</sub></code> is **minimized**. Note that `i` may equal `j`.

Return _an array of **right interval** indices for each interval `i`_. If no **right interval** exists for interval `i`, then put `-1` at index `i`.

**Example 1:**

**Input:** intervals = \[\[1,2]]

**Output:** [-1]

**Explanation:** There is only one interval in the collection, so it outputs -1.

**Example 2:**

**Input:** intervals = \[\[3,4],[2,3],[1,2]]

**Output:** [-1,0,1]

**Explanation:** There is no right interval for [3,4]. 

The right interval for [2,3] is [3,4] since start<sub>0</sub> = 3 is the smallest start that is >= end<sub>1</sub> = 3. 

The right interval for [1,2] is [2,3] since start<sub>1</sub> = 2 is the smallest start that is >= end<sub>2</sub> = 2.

**Example 3:**

**Input:** intervals = \[\[1,4],[2,3],[3,4]]

**Output:** [-1,2,-1]

**Explanation:** There is no right interval for [1,4] and [3,4]. The right interval for [2,3] is [3,4] since start<sub>2</sub> = 3 is the smallest start that is >= end<sub>1</sub> = 3.

**Constraints:**

*   <code>1 <= intervals.length <= 2 * 10<sup>4</sup></code>
*   `intervals[i].length == 2`
*   <code>-10<sup>6</sup> <= start<sub>i</sub> <= end<sub>i</sub> <= 10<sup>6</sup></code>
*   The start point of each interval is **unique**.

## Solution

```kotlin
import java.util.function.Function

class Solution {
    private fun findminmax(num: Array<IntArray>): IntArray {
        var min = num[0][0]
        var max = num[0][0]
        for (i in 1 until num.size) {
            min = Math.min(min, num[i][0])
            max = Math.max(max, num[i][0])
        }
        return intArrayOf(min, max)
    }

    fun findRightInterval(intervals: Array<IntArray>): IntArray {
        if (intervals.size <= 1) {
            return intArrayOf(-1)
        }
        val n = intervals.size
        val result = IntArray(n)
        val map: MutableMap<Int, Int?> = HashMap()
        for (i in 0 until n) {
            map[intervals[i][0]] = i
        }
        val minmax = findminmax(intervals)
        for (i in minmax[1] - 1 downTo minmax[0] + 1) {
            map.computeIfAbsent(i, Function { k: Int -> map[k + 1] })
        }
        for (i in 0 until n) {
            result[i] = map.getOrDefault(intervals[i][1], -1)!!
        }
        return result
    }
}
```