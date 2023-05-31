[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1162\. As Far from Land as Possible

Medium

Given an `n x n` `grid` containing only values `0` and `1`, where `0` represents water and `1` represents land, find a water cell such that its distance to the nearest land cell is maximized, and return the distance. If no land or water exists in the grid, return `-1`.

The distance used in this problem is the Manhattan distance: the distance between two cells `(x0, y0)` and `(x1, y1)` is `|x0 - x1| + |y0 - y1|`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/05/03/1336_ex1.JPG)

**Input:** grid = \[\[1,0,1],[0,0,0],[1,0,1]]

**Output:** 2

**Explanation:** The cell (1, 1) is as far as possible from all the land with distance 2.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/05/03/1336_ex2.JPG)

**Input:** grid = \[\[1,0,0],[0,0,0],[0,0,0]]

**Output:** 4

**Explanation:** The cell (2, 2) is as far as possible from all the land with distance 4.

**Constraints:**

*   `n == grid.length`
*   `n == grid[i].length`
*   `1 <= n <= 100`
*   `grid[i][j]` is `0` or `1`

## Solution

```kotlin
import java.util.LinkedList
import java.util.Objects
import java.util.Queue

class Solution {
    fun maxDistance(grid: Array<IntArray>): Int {
        val q: Queue<IntArray> = LinkedList()
        val n = grid.size
        val m = grid[0].size
        val vis = Array(n) { BooleanArray(m) }
        for (i in 0 until n) {
            for (j in 0 until m) {
                if (grid[i][j] == 1) {
                    q.add(intArrayOf(i, j))
                    vis[i][j] = true
                }
            }
        }
        if (q.isEmpty() || q.size == n * m) {
            return -1
        }
        val dir = intArrayOf(-1, 0, 1, 0, -1)
        var maxDistance = 0
        var level = 1
        while (q.isNotEmpty()) {
            val size = q.size
            for (i in 0 until size) {
                val top = q.poll()
                val currX = Objects.requireNonNull(top)[0]
                val currY = top[1]
                for (j in 0 until dir.size - 1) {
                    val x = currX + dir[j]
                    val y = currY + dir[j + 1]
                    if (x >= 0 && x != n && y >= 0 && y != n && !vis[x][y]) {
                        maxDistance = Math.max(maxDistance, level)
                        vis[x][y] = true
                        q.add(intArrayOf(x, y))
                    }
                }
            }
            level++
        }
        return maxDistance
    }
}
```