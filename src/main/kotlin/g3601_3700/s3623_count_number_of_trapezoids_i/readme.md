[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3623\. Count Number of Trapezoids I

Medium

You are given a 2D integer array `points`, where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents the coordinates of the <code>i<sup>th</sup></code> point on the Cartesian plane.

A **horizontal** **trapezoid** is a convex quadrilateral with **at least one pair** of horizontal sides (i.e. parallel to the x-axis). Two lines are parallel if and only if they have the same slope.

Return the _number of unique_ **_horizontal_ _trapezoids_** that can be formed by choosing any four distinct points from `points`.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** points = \[\[1,0],[2,0],[3,0],[2,2],[3,2]]

**Output:** 3

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/05/01/desmos-graph-6.png) ![](https://assets.leetcode.com/uploads/2025/05/01/desmos-graph-7.png) ![](https://assets.leetcode.com/uploads/2025/05/01/desmos-graph-8.png)

There are three distinct ways to pick four points that form a horizontal trapezoid:

*   Using points `[1,0]`, `[2,0]`, `[3,2]`, and `[2,2]`.
*   Using points `[2,0]`, `[3,0]`, `[3,2]`, and `[2,2]`.
*   Using points `[1,0]`, `[3,0]`, `[3,2]`, and `[2,2]`.

**Example 2:**

**Input:** points = \[\[0,0],[1,0],[0,1],[2,1]]

**Output:** 1

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/04/29/desmos-graph-5.png)

There is only one horizontal trapezoid that can be formed.

**Constraints:**

*   <code>4 <= points.length <= 10<sup>5</sup></code>
*   <code>–10<sup>8</sup> <= x<sub>i</sub>, y<sub>i</sub> <= 10<sup>8</sup></code>
*   All points are pairwise distinct.

## Solution

```kotlin
class Solution {
    fun countTrapezoids(points: Array<IntArray>): Int {
        val mod = 1000000007
        val inv = 500000004L
        val map: MutableMap<Int, Int> = HashMap<Int, Int>(points.size)
        for (p in points) {
            map.merge(p[1], 1) { a: Int, b: Int -> Integer.sum(a, b) }
        }
        var sum = 0L
        var sumPairs = 0L
        for (num in map.values) {
            if (num > 1) {
                val pairs = (num.toLong() * (num - 1) / 2) % mod
                sum = (sum + pairs) % mod
                sumPairs = (sumPairs + pairs * pairs % mod) % mod
            }
        }
        var res = (sum * sum % mod - sumPairs + mod) % mod
        res = (res * inv) % mod
        return res.toInt()
    }
}
```