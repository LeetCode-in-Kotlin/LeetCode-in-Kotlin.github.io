[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3625\. Count Number of Trapezoids II

Hard

You are given a 2D integer array `points` where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents the coordinates of the <code>i<sup>th</sup></code> point on the Cartesian plane.

Return _the number of unique_ _trapezoids_ that can be formed by choosing any four distinct points from `points`.

A **trapezoid** is a convex quadrilateral with **at least one pair** of parallel sides. Two lines are parallel if and only if they have the same slope.

**Example 1:**

**Input:** points = \[\[-3,2],[3,0],[2,3],[3,2],[2,-3]]

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/04/29/desmos-graph-4.png) ![](https://assets.leetcode.com/uploads/2025/04/29/desmos-graph-3.png)

There are two distinct ways to pick four points that form a trapezoid:

*   The points `[-3,2], [2,3], [3,2], [2,-3]` form one trapezoid.
*   The points `[2,3], [3,2], [3,0], [2,-3]` form another trapezoid.

**Example 2:**

**Input:** points = \[\[0,0],[1,0],[0,1],[2,1]]

**Output:** 1

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/04/29/desmos-graph-5.png)

There is only one trapezoid which can be formed.

**Constraints:**

*   `4 <= points.length <= 500`
*   <code>–1000 <= x<sub>i</sub>, y<sub>i</sub> <= 1000</code>
*   All points are pairwise distinct.

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    private class Slope(var dx: Int, var dy: Int) {
        override fun equals(other: Any?): Boolean {
            if (this === other) {
                return true
            }
            if (other !is Slope) {
                return false
            }
            val s: Slope = other
            return dx == s.dx && dy == s.dy
        }

        override fun hashCode(): Int {
            return dx * 1000003 xor dy
        }
    }

    private class Pair(var a: Int, var b: Int) {
        override fun equals(other: Any?): Boolean {
            if (this === other) {
                return true
            }
            if (other !is Pair) {
                return false
            }
            val p = other
            return a == p.a && b == p.b
        }

        override fun hashCode(): Int {
            return a * 1000003 xor b
        }
    }

    fun countTrapezoids(points: Array<IntArray>): Int {
        val n = points.size
        val slopeLines: MutableMap<Slope, MutableMap<Long, Int>> = HashMap()
        val midpointSlopes: MutableMap<Pair, MutableMap<Slope, Int>> = HashMap<Pair, MutableMap<Slope, Int>>()
        for (i in 0..<n) {
            val x1 = points[i][0]
            val y1 = points[i][1]
            for (j in i + 1..<n) {
                val x2 = points[j][0]
                val y2 = points[j][1]
                var dx = x2 - x1
                var dy = y2 - y1
                val g = gcd(abs(dx), abs(dy))
                dx /= g
                dy /= g
                if (dx < 0 || (dx == 0 && dy < 0)) {
                    dx = -dx
                    dy = -dy
                }
                val nx = -dy
                val ny = dx
                val lineId = nx.toLong() * x1 + ny.toLong() * y1
                val slopeKey = Slope(dx, dy)
                slopeLines
                    .computeIfAbsent(slopeKey) { _: Slope -> HashMap() }
                    .merge(lineId, 1) { a: Int, b: Int -> Integer.sum(a, b) }
                val mx = x1 + x2
                val my = y1 + y2
                val mid = Pair(mx, my)
                midpointSlopes
                    .computeIfAbsent(mid) { _: Pair -> HashMap() }
                    .merge(slopeKey, 1) { a: Int, b: Int -> Integer.sum(a, b) }
            }
        }
        var trapezoidsRaw: Long = 0
        for (lines in slopeLines.values) {
            if (lines.size < 2) {
                continue
            }
            var s: Long = 0
            var s2: Long = 0
            for (line in lines.values) {
                s += line.toLong()
                s2 += line.toLong() * line
            }
            trapezoidsRaw += (s * s - s2) / 2
        }
        var parallelograms: Long = 0
        for (mp in midpointSlopes.values) {
            if (mp.size < 2) {
                continue
            }
            var s: Long = 0
            var s2: Long = 0
            for (num in mp.values) {
                s += num.toLong()
                s2 += num.toLong() * num
            }
            parallelograms += (s * s - s2) / 2
        }
        val res = trapezoidsRaw - parallelograms
        return if (res > Int.Companion.MAX_VALUE) Int.Companion.MAX_VALUE else res.toInt()
    }

    private fun gcd(a: Int, b: Int): Int {
        var a = a
        var b = b
        while (b != 0) {
            val t = a % b
            a = b
            b = t
        }
        return if (a == 0) 1 else a
    }
}
```