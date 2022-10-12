[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 149\. Max Points on a Line

Hard

Given an array of `points` where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents a point on the **X-Y** plane, return _the maximum number of points that lie on the same straight line_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/25/plane1.jpg)

**Input:** points = \[\[1,1],[2,2],[3,3]]

**Output:** 3

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/25/plane2.jpg)

**Input:** points = \[\[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]

**Output:** 4

**Constraints:**

*   `1 <= points.length <= 300`
*   `points[i].length == 2`
*   <code>-10<sup>4</sup> <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>4</sup></code>
*   All the `points` are **unique**.

## Solution

```kotlin
class Solution {
    fun maxPoints(points: Array<IntArray>): Int {
        if (points.size < 2) {
            return points.size
        }
        var max = 0
        for (i in points.indices) {
            for (j in i + 1 until points.size) {
                val x = points[j][0] - points[i][0]
                val y = points[j][1] - points[i][1]
                val c = x * points[j][1] - y * points[j][0]
                var count = 2
                for (k in j + 1 until points.size) {
                    if (c == x * points[k][1] - y * points[k][0]) {
                        count++
                    }
                }
                max = Math.max(count, max)
            }
        }
        return max
    }
}
```