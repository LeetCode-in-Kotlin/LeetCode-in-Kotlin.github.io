[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 934\. Shortest Bridge

Medium

You are given an `n x n` binary matrix `grid` where `1` represents land and `0` represents water.

An **island** is a 4-directionally connected group of `1`'s not connected to any other `1`'s. There are **exactly two islands** in `grid`.

You may change `0`'s to `1`'s to connect the two islands to form **one island**.

Return _the smallest number of_ `0`_'s you must flip to connect the two islands_.

**Example 1:**

**Input:** grid = \[\[0,1],[1,0]]

**Output:** 1

**Example 2:**

**Input:** grid = \[\[0,1,0],[0,0,0],[0,0,1]]

**Output:** 2

**Example 3:**

**Input:** grid = \[\[1,1,1,1,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,0,1],[1,1,1,1,1]]

**Output:** 1

**Constraints:**

*   `n == grid.length == grid[i].length`
*   `2 <= n <= 100`
*   `grid[i][j]` is either `0` or `1`.
*   There are exactly two islands in `grid`.

## Solution

```kotlin
class Solution {
    private class Pair(var x: Int, var y: Int)

    private val dirs = arrayOf(intArrayOf(1, 0), intArrayOf(0, 1), intArrayOf(-1, 0), intArrayOf(0, -1))

    fun shortestBridge(grid: Array<IntArray>): Int {
        val q: ArrayDeque<Pair> = ArrayDeque()
        var flag = false
        val visited = Array(grid.size) {
            BooleanArray(
                grid[0].size
            )
        }
        var i = 0
        while (i < grid.size && !flag) {
            var j = 0
            while (j < grid[i].size && !flag) {
                if (grid[i][j] == 1) {
                    dfs(grid, i, j, visited, q)
                    flag = true
                }
                j++
            }
            i++
        }
        var level = -1
        while (!q.isEmpty()) {
            var size: Int = q.size
            level++
            while (size-- > 0) {
                val rem = q.removeFirst()
                for (dir in dirs) {
                    val newrow = rem.x + dir[0]
                    val newcol = rem.y + dir[1]
                    if (newrow >= 0 && newcol >= 0 && newrow < grid.size && newcol < grid[0].size &&
                        !visited[newrow][newcol]
                    ) {
                        if (grid[newrow][newcol] == 1) {
                            return level
                        }
                        q.add(Pair(newrow, newcol))
                        visited[newrow][newcol] = true
                    }
                }
            }
        }
        return -1
    }

    private fun dfs(grid: Array<IntArray>, row: Int, col: Int, visited: Array<BooleanArray>, q: ArrayDeque<Pair>) {
        visited[row][col] = true
        q.add(Pair(row, col))
        for (dir in dirs) {
            val newrow = row + dir[0]
            val newcol = col + dir[1]
            if (newrow >= 0 && newcol >= 0 && newrow < grid.size && newcol < grid[0].size &&
                !visited[newrow][newcol] && grid[newrow][newcol] == 1
            ) {
                dfs(grid, newrow, newcol, visited, q)
            }
        }
    }
}
```