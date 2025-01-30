[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3414\. Maximum Score of Non-overlapping Intervals

Hard

You are given a 2D integer array `intervals`, where <code>intervals[i] = [l<sub>i</sub>, r<sub>i</sub>, weight<sub>i</sub>]</code>. Interval `i` starts at position <code>l<sub>i</sub></code> and ends at <code>r<sub>i</sub></code>, and has a weight of <code>weight<sub>i</sub></code>. You can choose _up to_ 4 **non-overlapping** intervals. The **score** of the chosen intervals is defined as the total sum of their weights.

Return the **lexicographically smallest** array of at most 4 indices from `intervals` with **maximum** score, representing your choice of non-overlapping intervals.

Two intervals are said to be **non-overlapping** if they do not share any points. In particular, intervals sharing a left or right boundary are considered overlapping.

An array `a` is **lexicographically smaller** than an array `b` if in the first position where `a` and `b` differ, array `a` has an element that is less than the corresponding element in `b`.   
 If the first `min(a.length, b.length)` elements do not differ, then the shorter array is the lexicographically smaller one.

**Example 1:**

**Input:** intervals = \[\[1,3,2],[4,5,2],[1,5,5],[6,9,3],[6,7,1],[8,9,1]]

**Output:** [2,3]

**Explanation:**

You can choose the intervals with indices 2, and 3 with respective weights of 5, and 3.

**Example 2:**

**Input:** intervals = \[\[5,8,1],[6,7,7],[4,7,3],[9,10,6],[7,8,2],[11,14,3],[3,5,5]]

**Output:** [1,3,5,6]

**Explanation:**

You can choose the intervals with indices 1, 3, 5, and 6 with respective weights of 7, 6, 3, and 5.

**Constraints:**

*   <code>1 <= intevals.length <= 5 * 10<sup>4</sup></code>
*   `intervals[i].length == 3`
*   <code>intervals[i] = [l<sub>i</sub>, r<sub>i</sub>, weight<sub>i</sub>]</code>
*   <code>1 <= l<sub>i</sub> <= r<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= weight<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun maximumWeight(intervals: List<List<Int>>): IntArray {
        val n = intervals.size
        val ns = intervals.mapIndexed { index, li -> intArrayOf(li[0], li[1], li[2], index) }.toTypedArray()
        ns.sortBy { it[0] }
        var dp1 = Array<IntArray>(n) { IntArray(0) }
        var dp = LongArray(n)
        repeat(4) {
            val dp3 = Array<IntArray>(n) { IntArray(0) }
            val dp2 = LongArray(n)
            dp3[n - 1] = intArrayOf(ns[n - 1][3])
            dp2[n - 1] = ns[n - 1][2].toLong()
            for (i in n - 2 downTo 0) {
                var l = i + 1
                var r = n - 1
                while (l <= r) {
                    val mid = (l + r) shr 1
                    if (ns[mid][0] > ns[i][1]) {
                        r = mid - 1
                    } else {
                        l = mid + 1
                    }
                }
                dp2[i] = ns[i][2] + (if (l < n) dp[l] else 0)
                if (i + 1 < n && dp2[i + 1] > dp2[i]) {
                    dp2[i] = dp2[i + 1]
                    dp3[i] = dp3[i + 1]
                } else {
                    if (l < n) {
                        dp3[i] = IntArray(dp1[l].size + 1)
                        dp3[i][0] = ns[i][3]
                        for (j in dp1[l].indices) {
                            dp3[i][j + 1] = dp1[l][j]
                        }
                        dp3[i].sort()
                    } else {
                        dp3[i] = intArrayOf(ns[i][3])
                    }
                    if (i + 1 < n && dp2[i + 1] == dp2[i] && check(dp3[i], dp3[i + 1]) > 0) {
                        dp3[i] = dp3[i + 1]
                    }
                }
            }
            dp = dp2
            dp1 = dp3
        }
        return dp1[0]
    }

    private fun check(ns1: IntArray, ns2: IntArray): Int {
        var i = 0
        while (i < ns1.size && i < ns2.size) {
            if (ns1[i] < ns2[i]) {
                return -1
            } else if (ns1[i] > ns2[i]) {
                return 1
            }
            i++
        }
        return 0
    }
}
```