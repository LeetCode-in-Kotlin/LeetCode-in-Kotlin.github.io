[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 695\. Max Area of Island

Medium

You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value `1` in the island.

Return _the maximum **area** of an island in_ `grid`. If there is no island, return `0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

**Input:** grid = \[\[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]

**Output:** 6

**Explanation:** The answer is not 11, because the island must be connected 4-directionally.

**Example 2:**

**Input:** grid = \[\[0,0,0,0,0,0,0,0]]

**Output:** 0

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 50`
*   `grid[i][j]` is either `0` or `1`.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun maxAreaOfIsland(grid: Array<IntArray>?): Int {
        if (grid.isNullOrEmpty()) {
            return 0
        }
        val m = grid.size
        val n = grid[0].size
        var max = 0
        for (i in 0 until m) {
            for (j in 0 until n) {
                if (grid[i][j] == 1) {
                    val area = dfs(grid, i, j, m, n, 0)
                    max = Math.max(area, max)
                }
            }
        }
        return max
    }

    private fun dfs(grid: Array<IntArray>, i: Int, j: Int, m: Int, n: Int, area: Int): Int {
        var area = area
        if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == 0) {
            return area
        }
        grid[i][j] = 0
        area++
        area = dfs(grid, i + 1, j, m, n, area)
        area = dfs(grid, i, j + 1, m, n, area)
        area = dfs(grid, i - 1, j, m, n, area)
        area = dfs(grid, i, j - 1, m, n, area)
        return area
    }
}
```