[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1631\. Path With Minimum Effort

Medium

You are a hiker preparing for an upcoming hike. You are given `heights`, a 2D array of size `rows x columns`, where `heights[row][col]` represents the height of cell `(row, col)`. You are situated in the top-left cell, `(0, 0)`, and you hope to travel to the bottom-right cell, `(rows-1, columns-1)` (i.e., **0-indexed**). You can move **up**, **down**, **left**, or **right**, and you wish to find a route that requires the minimum **effort**.

A route's **effort** is the **maximum absolute difference** in heights between two consecutive cells of the route.

Return _the minimum **effort** required to travel from the top-left cell to the bottom-right cell._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/04/ex1.png)

**Input:** heights = \[\[1,2,2],[3,8,2],[5,3,5]]

**Output:** 2

**Explanation:** The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells. This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/04/ex2.png)

**Input:** heights = \[\[1,2,3],[3,8,4],[5,3,5]]

**Output:** 1

**Explanation:** The route of [1,2,3,4,5] has a maximum absolute difference of 1 in consecutive cells, which is better than route [1,3,5,3,5].

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/10/04/ex3.png)

**Input:** heights = \[\[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]

**Output:** 0

**Explanation:** This route does not require any effort.

**Constraints:**

*   `rows == heights.length`
*   `columns == heights[i].length`
*   `1 <= rows, columns <= 100`
*   <code>1 <= heights[i][j] <= 10<sup>6</sup></code>

## Solution

```kotlin
import java.util.PriorityQueue
import kotlin.math.abs

class Solution {
    private class Pair internal constructor(var row: Int, var col: Int, var diff: Int) : Comparable<Pair> {
        override fun compareTo(other: Pair): Int {
            return diff - other.diff
        }
    }

    fun minimumEffortPath(heights: Array<IntArray>): Int {
        val n = heights.size
        val m = heights[0].size
        val pq = PriorityQueue<Pair>()
        pq.add(Pair(0, 0, 0))
        val vis = Array(n) { BooleanArray(m) }
        val dx = intArrayOf(-1, 0, 1, 0)
        val dy = intArrayOf(0, 1, 0, -1)
        var min = Int.MAX_VALUE
        while (pq.isNotEmpty()) {
            val p = pq.remove()
            val row = p.row
            val col = p.col
            val diff = p.diff
            if (vis[row][col]) {
                continue
            }
            vis[row][col] = true
            if (row == n - 1 && col == m - 1) {
                min = min.coerceAtMost(diff)
            }
            for (i in 0..3) {
                val r = row + dx[i]
                val c = col + dy[i]
                if (r < 0 || c < 0 || r >= n || c >= m || vis[r][c]) {
                    continue
                }
                pq.add(
                    Pair(
                        r,
                        c,
                        diff.coerceAtLeast(
                            abs(
                                heights[r][c] - heights[row][col],
                            ),
                        ),
                    ),
                )
            }
        }
        return min
    }
}
```