[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3235\. Check if the Rectangle Corner Is Reachable

Hard

You are given two positive integers `X` and `Y`, and a 2D array `circles`, where <code>circles[i] = [x<sub>i</sub>, y<sub>i</sub>, r<sub>i</sub>]</code> denotes a circle with center at <code>(x<sub>i</sub>, y<sub>i</sub>)</code> and radius <code>r<sub>i</sub></code>.

There is a rectangle in the coordinate plane with its bottom left corner at the origin and top right corner at the coordinate `(X, Y)`. You need to check whether there is a path from the bottom left corner to the top right corner such that the **entire path** lies inside the rectangle, **does not** touch or lie inside **any** circle, and touches the rectangle **only** at the two corners.

Return `true` if such a path exists, and `false` otherwise.

**Example 1:**

**Input:** X = 3, Y = 4, circles = \[\[2,1,1]]

**Output:** true

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/05/18/example2circle1.png)

The black curve shows a possible path between `(0, 0)` and `(3, 4)`.

**Example 2:**

**Input:** X = 3, Y = 3, circles = \[\[1,1,2]]

**Output:** false

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/05/18/example1circle.png)

No path exists from `(0, 0)` to `(3, 3)`.

**Example 3:**

**Input:** X = 3, Y = 3, circles = \[\[2,1,1],[1,2,1]]

**Output:** false

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/05/18/example0circle.png)

No path exists from `(0, 0)` to `(3, 3)`.

**Example 4:**

**Input:** X = 4, Y = 4, circles = \[\[5,5,1]]

**Output:** true

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/31/rectangleexample5.png)

**Constraints:**

*   <code>3 <= X, Y <= 10<sup>9</sup></code>
*   `1 <= circles.length <= 1000`
*   `circles[i].length == 3`
*   <code>1 <= x<sub>i</sub>, y<sub>i</sub>, r<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.pow
import kotlin.math.sqrt

class Solution {
    fun canReachCorner(x: Int, y: Int, circles: Array<IntArray>): Boolean {
        val n = circles.size
        val ds = DisjointSet(n + 5)

        // Special nodes for boundaries
        val leftBoundary = n + 3
        val topBoundary = n
        val rightBoundary = n + 1
        val bottomBoundary = n + 2

        var i = 0
        for (it in circles) {
            val xi = it[0]
            val yi = it[1]
            val ri = it[2]
            if (yi - ri >= y || xi - ri >= x) {
                continue
            }
            if (((xi > (x + y) || yi > y) && (xi > x || yi > x + y))) {
                continue
            }
            if (xi <= ri) {
                ds.dsu(i, leftBoundary)
            }
            if (yi <= ri) {
                ds.dsu(i, topBoundary)
            }
            if (x - xi <= ri) {
                ds.dsu(i, rightBoundary)
            }
            if (y - yi <= ri) {
                ds.dsu(i, bottomBoundary)
            }
            i++
        }

        // Union circles that overlap
        i = 0
        while (i < n) {
            val x1 = circles[i][0]
            val y1 = circles[i][1]
            val r1 = circles[i][2]
            if (y1 - r1 >= y || x1 - r1 >= x) {
                i++
                continue
            }
            if (((x1 > (x + y) || y1 > y) && (x1 > x || y1 > x + y))) {
                i++
                continue
            }

            for (j in i + 1 until n) {
                val x2 = circles[j][0]
                val y2 = circles[j][1]
                val r2 = circles[j][2]
                val dist = sqrt(
                    (x1 - x2.toDouble()).pow(2.0) + (y1 - y2.toDouble()).pow(2.0)
                )
                if (dist <= (r1 + r2)) {
                    ds.dsu(i, j)
                }
            }
            i++
        }

        // Check if left is connected to right or top is connected to bottom
        if (ds.findUpar(leftBoundary) == ds.findUpar(rightBoundary) ||
            ds.findUpar(leftBoundary) == ds.findUpar(topBoundary)
        ) {
            return false
        }
        return (
            ds.findUpar(bottomBoundary) != ds.findUpar(rightBoundary) &&
                ds.findUpar(bottomBoundary) != ds.findUpar(topBoundary)
            )
    }

    private class DisjointSet(n: Int) {
        private val parent: IntArray
        private val size = IntArray(n + 1)

        init {
            size.fill(1)
            parent = IntArray(n + 1)
            for (i in 0..n) {
                parent[i] = i
            }
        }

        fun findUpar(u: Int): Int {
            if (u == parent[u]) {
                return u
            }
            parent[u] = findUpar(parent[u])
            return parent[u]
        }

        fun dsu(u: Int, v: Int) {
            val ulpu = findUpar(u)
            val ulpv = findUpar(v)
            if (ulpv == ulpu) {
                return
            }
            if (size[ulpu] < size[ulpv]) {
                parent[ulpu] = ulpv
                size[ulpv] += size[ulpu]
            } else {
                parent[ulpv] = ulpu
                size[ulpu] += size[ulpv]
            }
        }
    }
}
```