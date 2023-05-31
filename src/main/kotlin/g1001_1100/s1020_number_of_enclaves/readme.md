[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1020\. Number of Enclaves

Medium

You are given an `m x n` binary matrix `grid`, where `0` represents a sea cell and `1` represents a land cell.

A **move** consists of walking from one land cell to another adjacent (**4-directionally**) land cell or walking off the boundary of the `grid`.

Return _the number of land cells in_ `grid` _for which we cannot walk off the boundary of the grid in any number of **moves**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/18/enclaves1.jpg)

**Input:** grid = \[\[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]

**Output:** 3

**Explanation:** There are three 1s that are enclosed by 0s, and one 1 that is not enclosed because its on the boundary.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/18/enclaves2.jpg)

**Input:** grid = \[\[0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0]]

**Output:** 0

**Explanation:** All 1s are either on the boundary or can reach the boundary.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 500`
*   `grid[i][j]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    fun numEnclaves(grid: Array<IntArray>): Int {
        val visited = Array(grid.size) {
            BooleanArray(
                grid[0].size
            )
        }
        for (i in grid.indices) {
            for (j in grid[0].indices) {
                if (grid[i][j] == 1 && (i == 0 || j == 0 || i == grid.size - 1 || j == grid[0].size - 1)) {
                    move(grid, i, j, visited)
                }
            }
        }
        var count = 0
        for (i in 1 until visited.size - 1) {
            for (j in 1 until visited[0].size - 1) {
                if (!visited[i][j] && grid[i][j] == 1) count++
            }
        }
        return count
    }

    companion object {
        fun move(g: Array<IntArray>, i: Int, j: Int, b: Array<BooleanArray>) {
            if (i < 0 || j < 0 || i == g.size || j == g[0].size || g[i][j] == 0 || b[i][j]) return
            b[i][j] = true
            move(g, i + 1, j, b)
            move(g, i - 1, j, b)
            move(g, i, j - 1, b)
            move(g, i, j + 1, b)
        }
    }
}
```