[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 963\. Minimum Area Rectangle II

Medium

You are given an array of points in the **X-Y** plane `points` where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.

Return _the minimum area of any rectangle formed from these points, with sides **not necessarily parallel** to the X and Y axes_. If there is not any such rectangle, return `0`.

Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/21/1a.png)

**Input:** points = \[\[1,2],[2,1],[1,0],[0,1]]

**Output:** 2.00000

**Explanation:** The minimum area rectangle occurs at [1,2],[2,1],[1,0],[0,1], with an area of 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/22/2.png)

**Input:** points = \[\[0,1],[2,1],[1,1],[1,0],[2,0]]

**Output:** 1.00000

**Explanation:** The minimum area rectangle occurs at [1,0],[1,1],[2,1],[2,0], with an area of 1.

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/22/3.png)

**Input:** points = \[\[0,3],[1,2],[3,1],[1,3],[2,1]]

**Output:** 0

**Explanation:** There is no possible rectangle to form from these points.

**Constraints:**

*   `1 <= points.length <= 50`
*   `points[i].length == 2`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= 4 * 10<sup>4</sup></code>
*   All the given points are **unique**.

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun minAreaFreeRect(points: Array<IntArray>): Double {
        val map: MutableMap<Int, MutableSet<Int>> = HashMap()
        var area: Double
        for (point in points) {
            map.putIfAbsent(point[0], HashSet())
            map.getValue(point[0]).add(point[1])
        }
        var minArea = Double.MAX_VALUE
        val n = points.size
        for (i in 0 until n - 2) {
            for (j in i + 1 until n - 1) {
                val dx1 = points[j][0] - points[i][0]
                val dy1 = points[j][1] - points[i][1]
                // get the 3rd point
                for (k in j + 1 until n) {
                    val dx2 = points[k][0] - points[i][0]
                    val dy2 = points[k][1] - points[i][1]
                    if (dx1 * dx2 + dy1 * dy2 != 0) {
                        continue
                    }
                    // find the 4th point
                    val x = dx1 + points[k][0]
                    val y = dy1 + points[k][1]
                    area = calculateArea(points, i, j, k)
                    if (area >= minArea) {
                        continue
                    }
                    // 4th point exists
                    if (map[x] != null && map.getValue(x).contains(y)) {
                        minArea = area
                    }
                }
            }
        }
        return if (minArea == Double.MAX_VALUE) 0.0 else minArea
    }

    private fun calculateArea(points: Array<IntArray>, i: Int, j: Int, k: Int): Double {
        val first = points[i]
        val second = points[j]
        val third = points[k]
        return abs(
            first[0] * (second[1] - third[1]) + second[0] * (third[1] - first[1]) + third[0] * (first[1] - second[1]),
        ).toDouble()
    }
}
```