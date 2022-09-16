[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 200\. Number of Islands

Medium

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

**Input:** grid = [ 

["1","1","1","1","0"], 

["1","1","0","1","0"], 

["1","1","0","0","0"], 

["0","0","0","0","0"] 

]

**Output:** 1

**Example 2:**

**Input:** grid = [ 

["1","1","0","0","0"], 

["1","1","0","0","0"], 

["0","0","1","0","0"], 

["0","0","0","1","1"] 

]

**Output:** 3

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 300`
*   `grid[i][j]` is `'0'` or `'1'`.

## Solution

```kotlin
class Solution {
    fun numIslands(grid: Array<CharArray>): Int {
        var islands = 0
        if (grid.isNotEmpty() && grid[0].isNotEmpty()) {
            for (i in grid.indices) {
                for (j in grid[0].indices) {
                    if (grid[i][j] == '1') {
                        dfs(grid, i, j)
                        islands++
                    }
                }
            }
        }
        return islands
    }

    private fun dfs(grid: Array<CharArray>, x: Int, y: Int) {
        if (x < 0 || grid.size <= x || y < 0 || grid[0].size <= y || grid[x][y] != '1') {
            return
        }
        grid[x][y] = 'x'
        dfs(grid, x + 1, y)
        dfs(grid, x - 1, y)
        dfs(grid, x, y + 1)
        dfs(grid, x, y - 1)
    }
}
```