[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3464\. Maximize the Distance Between Points on a Square

Hard

You are given an integer `side`, representing the edge length of a square with corners at `(0, 0)`, `(0, side)`, `(side, 0)`, and `(side, side)` on a Cartesian plane.

You are also given a **positive** integer `k` and a 2D integer array `points`, where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents the coordinate of a point lying on the **boundary** of the square.

You need to select `k` elements among `points` such that the **minimum** Manhattan distance between any two points is **maximized**.

Return the **maximum** possible **minimum** Manhattan distance between the selected `k` points.

The Manhattan Distance between two cells <code>(x<sub>i</sub>, y<sub>i</sub>)</code> and <code>(x<sub>j</sub>, y<sub>j</sub>)</code> is <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>.

**Example 1:**

**Input:** side = 2, points = \[\[0,2],[2,0],[2,2],[0,0]], k = 4

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/28/4080_example0_revised.png)

Select all four points.

**Example 2:**

**Input:** side = 2, points = \[\[0,0],[1,2],[2,0],[2,2],[2,1]], k = 4

**Output:** 1

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/28/4080_example1_revised.png)

Select the points `(0, 0)`, `(2, 0)`, `(2, 2)`, and `(2, 1)`.

**Example 3:**

**Input:** side = 2, points = \[\[0,0],[0,1],[0,2],[1,2],[2,0],[2,2],[2,1]], k = 5

**Output:** 1

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/01/28/4080_example2_revised.png)

Select the points `(0, 0)`, `(0, 1)`, `(0, 2)`, `(1, 2)`, and `(2, 2)`.

**Constraints:**

*   <code>1 <= side <= 10<sup>9</sup></code>
*   <code>4 <= points.length <= min(4 * side, 15 * 10<sup>3</sup>)</code>
*   `points[i] == [xi, yi]`
*   The input is generated such that:
    *   `points[i]` lies on the boundary of the square.
    *   All `points[i]` are **unique**.
*   `4 <= k <= min(25, points.length)`

## Solution

```kotlin
class Solution {
    fun maxDistance(side: Int, points: Array<IntArray>, k: Int): Int {
        val n = points.size
        val p = LongArray(n)
        for (i in 0..<n) {
            val x = points[i][0]
            val y = points[i][1]
            val c: Long
            if (y == 0) {
                c = x.toLong()
            } else if (x == side) {
                c = side + y.toLong()
            } else if (y == side) {
                c = 2L * side + (side - x)
            } else {
                c = 3L * side + (side - y)
            }
            p[i] = c
        }
        p.sort()
        val c = 4L * side
        val tot = 2 * n
        val dArr = LongArray(tot)
        for (i in 0..<n) {
            dArr[i] = p[i]
            dArr[i + n] = p[i] + c
        }
        var lo = 0
        var hi = 2 * side
        var ans = 0
        while (lo <= hi) {
            val mid = (lo + hi) ushr 1
            if (check(mid, dArr, n, k, c)) {
                ans = mid
                lo = mid + 1
            } else {
                hi = mid - 1
            }
        }
        return ans
    }

    private fun check(d: Int, dArr: LongArray, n: Int, k: Int, c: Long): Boolean {
        val len = dArr.size
        val nxt = IntArray(len)
        var j = 0
        for (i in 0..<len) {
            if (j < i + 1) {
                j = i + 1
            }
            while (j < len && dArr[j] < dArr[i] + d) {
                j++
            }
            nxt[i] = if (j < len) j else -1
        }
        for (i in 0..<n) {
            var cnt = 1
            var cur = i
            while (cnt < k) {
                val nx = nxt[cur]
                if (nx == -1 || nx >= i + n) {
                    break
                }
                cur = nx
                cnt++
            }
            if (cnt == k && (dArr[i] + c - dArr[cur]) >= d) {
                return true
            }
        }
        return false
    }
}
```