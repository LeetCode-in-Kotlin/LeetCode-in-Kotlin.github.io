[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3619\. Count Islands With Total Value Divisible by K

Medium

You are given an `m x n` matrix `grid` and a positive integer `k`. An **island** is a group of **positive** integers (representing land) that are **4-directionally** connected (horizontally or vertically).

The **total value** of an island is the sum of the values of all cells in the island.

Return the number of islands with a total value **divisible by** `k`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2025/03/06/example1griddrawio-1.png)

**Input:** grid = \[\[0,2,1,0,0],[0,5,0,0,5],[0,0,1,0,0],[0,1,4,7,0],[0,2,0,0,8]], k = 5

**Output:** 2

**Explanation:**

The grid contains four islands. The islands highlighted in blue have a total value that is divisible by 5, while the islands highlighted in red do not.

**Example 2:**

![](https://assets.leetcode.com/uploads/2025/03/06/example2griddrawio.png)

**Input:** grid = \[\[3,0,3,0], [0,3,0,3], [3,0,3,0]], k = 3

**Output:** 6

**Explanation:**

The grid contains six islands, each with a total value that is divisible by 3.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 1000`
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   <code>0 <= grid[i][j] <= 10<sup>6</sup></code>
*   <code>1 <= k <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    private var m = 0
    private var n = 0

    fun countIslands(grid: Array<IntArray>, k: Int): Int {
        var count = 0
        m = grid.size
        n = grid[0].size
        for (i in 0..<m) {
            for (j in 0..<n) {
                if (grid[i][j] != 0) {
                    val curr = dfs(i, j, grid)
                    if (curr % k == 0) {
                        count++
                    }
                }
            }
        }
        return count
    }

    private fun dfs(i: Int, j: Int, grid: Array<IntArray>): Int {
        if (i >= m || j >= n || i < 0 || j < 0 || grid[i][j] == 0) {
            return Int.Companion.MAX_VALUE
        }
        var count = grid[i][j]
        grid[i][j] = 0
        val x = dfs(i + 1, j, grid)
        val y = dfs(i, j + 1, grid)
        val a = dfs(i - 1, j, grid)
        val b = dfs(i, j - 1, grid)
        if (x != Int.Companion.MAX_VALUE) {
            count += x
        }
        if (y != Int.Companion.MAX_VALUE) {
            count += y
        }
        if (a != Int.Companion.MAX_VALUE) {
            count += a
        }
        if (b != Int.Companion.MAX_VALUE) {
            count += b
        }
        return count
    }
}
```