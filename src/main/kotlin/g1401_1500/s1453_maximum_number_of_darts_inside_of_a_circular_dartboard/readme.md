[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1453\. Maximum Number of Darts Inside of a Circular Dartboard

Hard

Alice is throwing `n` darts on a very large wall. You are given an array `darts` where <code>darts[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> is the position of the <code>i<sup>th</sup></code> dart that Alice threw on the wall.

Bob knows the positions of the `n` darts on the wall. He wants to place a dartboard of radius `r` on the wall so that the maximum number of darts that Alice throws lies on the dartboard.

Given the integer `r`, return _the maximum number of darts that can lie on the dartboard_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/04/29/sample_1_1806.png)

**Input:** darts = \[\[-2,0],[2,0],[0,2],[0,-2]], r = 2

**Output:** 4

**Explanation:** Circle dartboard with center in (0,0) and radius = 2 contain all points.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/04/29/sample_2_1806.png)

**Input:** darts = \[\[-3,0],[3,0],[2,6],[5,4],[0,9],[7,8]], r = 5

**Output:** 5

**Explanation:** Circle dartboard with center in (0,4) and radius = 5 contain all points except the point (7,8).

**Constraints:**

*   `1 <= darts.length <= 100`
*   `darts[i].length == 2`
*   <code>-10<sup>4</sup> <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>4</sup></code>
*   `1 <= r <= 5000`

## Solution

```kotlin
class Solution {
    private class Angle(var a: Double, var enter: Boolean) : Comparable<Angle> {
        override fun compareTo(other: Angle): Int {
            if (a > other.a) {
                return 1
            } else if (a < other.a) {
                return -1
            } else if (enter == other.enter) {
                return 0
            } else if (enter) {
                return -1
            }
            return 1
        }
    }

    private fun getPointsInside(i: Int, r: Double, n: Int, points: Array<IntArray>, dis: Array<DoubleArray>): Int {
        val angles: MutableList<Angle> = ArrayList(2 * n)
        for (j in 0 until n) {
            if (i != j && dis[i][j] <= 2 * r) {
                val b = Math.acos(dis[i][j] / (2 * r))
                val a = Math.atan2(
                    points[j][1] - points[i][1] * 1.0,
                    points[j][0] * 1.0 - points[i][0],
                )
                val alpha = a - b
                val beta = a + b
                angles.add(Angle(alpha, true))
                angles.add(Angle(beta, false))
            }
        }
        angles.sort()
        var count = 1
        var res = 1
        for (a in angles) {
            if (a.enter) {
                count++
            } else {
                count--
            }
            if (count > res) {
                res = count
            }
        }
        return res
    }

    fun numPoints(points: Array<IntArray>, r: Int): Int {
        val n = points.size
        val dis = Array(n) { DoubleArray(n) }
        for (i in 0 until n - 1) {
            for (j in i + 1 until n) {
                dis[j][i] = Math.sqrt(
                    Math.pow(points[i][0] * 1.0 - points[j][0], 2.0) +
                        Math.pow(points[i][1] * 1.0 - points[j][1], 2.0),
                )
                dis[i][j] = dis[j][i]
            }
        }
        var ans = 0
        for (i in 0 until n) {
            ans = Math.max(ans, getPointsInside(i, r.toDouble(), n, points, dis))
        }
        return ans
    }
}
```