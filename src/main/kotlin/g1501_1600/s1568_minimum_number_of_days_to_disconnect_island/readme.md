[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1568\. Minimum Number of Days to Disconnect Island

Hard

You are given an `m x n` binary grid `grid` where `1` represents land and `0` represents water. An **island** is a maximal **4-directionally** (horizontal or vertical) connected group of `1`'s.

The grid is said to be **connected** if we have **exactly one island**, otherwise is said **disconnected**.

In one day, we are allowed to change **any** single land cell `(1)` into a water cell `(0)`.

Return _the minimum number of days to disconnect the grid_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/12/24/land1.jpg)

**Input:** grid = \[\[0,1,1,0],[0,1,1,0],[0,0,0,0]]

**Output:** 2

**Explanation:** We need at least 2 days to get a disconnected grid. Change land grid[1][1] and grid[0][2] to water and get 2 disconnected island.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/12/24/land2.jpg)

**Input:** grid = \[\[1,1]]

**Output:** 2

**Explanation:** Grid of full water is also disconnected ([[1,1]] -> [[0,0]]), 0 islands.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 30`
*   `grid[i][j]` is either `0` or `1`.

## Solution

```kotlin
@Suppress("kotlin:S107")
class Solution {
    private val dirs = arrayOf(intArrayOf(0, 1), intArrayOf(0, -1), intArrayOf(1, 0), intArrayOf(-1, 0))
    fun minDays(grid: Array<IntArray>): Int {
        val m = grid.size
        val n = grid[0].size
        var numOfIslands = 0
        var hasArticulationPoint = false
        var color = 1
        var minIslandSize = m * n
        val time = Array(m) { IntArray(n) }
        val low = Array(m) { IntArray(n) }
        for (i in 0 until m) {
            for (j in 0 until n) {
                if (grid[i][j] == 1) {
                    numOfIslands++
                    color++
                    val articulationPoints: MutableList<Int> = ArrayList()
                    val islandSize = IntArray(1)
                    tarjan(i, j, -1, -1, 0, time, low, grid, articulationPoints, color, islandSize)
                    minIslandSize = Math.min(minIslandSize, islandSize[0])
                    if (articulationPoints.isNotEmpty()) {
                        hasArticulationPoint = true
                    }
                }
            }
        }
        if (numOfIslands >= 2) {
            return 0
        }
        if (numOfIslands == 0) {
            return 0
        }
        if (numOfIslands == 1 && minIslandSize == 1) {
            return 1
        }
        return if (hasArticulationPoint) 1 else 2
    }

    private fun tarjan(
        x: Int,
        y: Int,
        prex: Int,
        prey: Int,
        time: Int,
        times: Array<IntArray>,
        lows: Array<IntArray>,
        grid: Array<IntArray>,
        articulationPoints: MutableList<Int>,
        color: Int,
        islandSize: IntArray,
    ) {
        times[x][y] = time
        lows[x][y] = time
        grid[x][y] = color
        islandSize[0]++
        var children = 0
        for (dir in dirs) {
            val nx = x + dir[0]
            val ny = y + dir[1]
            if (nx < 0 || ny < 0 || nx >= grid.size || ny >= grid[0].size) {
                continue
            }
            if (grid[nx][ny] == 1) {
                children++
                tarjan(
                    nx,
                    ny,
                    x,
                    y,
                    time + 1,
                    times,
                    lows,
                    grid,
                    articulationPoints,
                    color,
                    islandSize,
                )
                lows[x][y] = Math.min(lows[x][y], lows[nx][ny])
                if (prex != -1 && lows[nx][ny] >= time) {
                    articulationPoints.add(x * grid.size + y)
                }
            } else if ((nx != prex || ny != prey) && grid[nx][ny] != 0) {
                lows[x][y] = Math.min(lows[x][y], times[nx][ny])
            }
        }
        if (prex == -1 && children > 1) {
            articulationPoints.add(x * grid.size + y)
        }
    }
}
```