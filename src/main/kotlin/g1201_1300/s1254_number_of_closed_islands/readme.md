[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1254\. Number of Closed Islands

Medium

Given a 2D `grid` consists of `0s` (land) and `1s` (water). An _island_ is a maximal 4-directionally connected group of `0s` and a _closed island_ is an island **totally** (all left, top, right, bottom) surrounded by `1s.`

Return the number of _closed islands_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/10/31/sample_3_1610.png)

**Input:** grid = \[\[1,1,1,1,1,1,1,0],[1,0,0,0,0,1,1,0],[1,0,1,0,1,1,1,0],[1,0,0,0,0,1,0,1],[1,1,1,1,1,1,1,0]]

**Output:** 2

**Explanation:** Islands in gray are closed because they are completely surrounded by water (group of 1s).

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/10/31/sample_4_1610.png)

**Input:** grid = \[\[0,0,1,0,0],[0,1,0,1,0],[0,1,1,1,0]]

**Output:** 1

**Example 3:**

**Input:** grid = \[\[1,1,1,1,1,1,1], 
                   [1,0,0,0,0,0,1], 
                   [1,0,1,1,1,0,1], 
                   [1,0,1,0,1,0,1], 
                   [1,0,1,1,1,0,1], 
                   [1,0,0,0,0,0,1], 
                   [1,1,1,1,1,1,1]]

**Output:** 2

**Constraints:**

*   `1 <= grid.length, grid[0].length <= 100`
*   `0 <= grid[i][j] <=1`

## Solution

```kotlin
class Solution {
    private var rows = 0
    private var cols = 0
    private var isLand = false
    fun closedIsland(grid: Array<IntArray>): Int {
        rows = grid.size
        cols = grid[0].size
        var result = 0
        for (r in 0 until rows) {
            for (c in 0 until cols) {
                if (grid[r][c] == 0) {
                    isLand = true
                    dfs(grid, r, c)
                    if (isLand) {
                        result++
                    }
                }
            }
        }
        return result
    }

    private fun dfs(grid: Array<IntArray>, r: Int, c: Int) {
        if (r == 0 || c == 0 || r == rows - 1 || c == cols - 1) {
            isLand = false
        }
        grid[r][c] = 'k'.code
        if (r > 0 && grid[r - 1][c] == 0) {
            dfs(grid, r - 1, c)
        }
        if (c > 0 && grid[r][c - 1] == 0) {
            dfs(grid, r, c - 1)
        }
        if (r < rows - 1 && grid[r + 1][c] == 0) {
            dfs(grid, r + 1, c)
        }
        if (c < cols - 1 && grid[r][c + 1] == 0) {
            dfs(grid, r, c + 1)
        }
    }
}
```