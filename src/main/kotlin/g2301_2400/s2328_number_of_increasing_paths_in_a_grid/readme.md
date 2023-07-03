[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2328\. Number of Increasing Paths in a Grid

Hard

You are given an `m x n` integer matrix `grid`, where you can move from a cell to any adjacent cell in all `4` directions.

Return _the number of **strictly** **increasing** paths in the grid such that you can start from **any** cell and end at **any** cell._ Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

Two paths are considered different if they do not have exactly the same sequence of visited cells.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/05/10/griddrawio-4.png)

**Input:** grid = \[\[1,1],[3,4]]

**Output:** 8

**Explanation:** The strictly increasing paths are:

- Paths with length 1: [1], [1], [3], [4].

- Paths with length 2: [1 -> 3], [1 -> 4], [3 -> 4].

- Paths with length 3: [1 -> 3 -> 4].

The total number of paths is 4 + 3 + 1 = 8.

**Example 2:**

**Input:** grid = \[\[1],[2]]

**Output:** 3

**Explanation:** The strictly increasing paths are:

- Paths with length 1: [1], [2].

- Paths with length 2: [1 -> 2].

The total number of paths is 2 + 1 = 3.

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 1000`
*   <code>1 <= m * n <= 10<sup>5</sup></code>
*   <code>1 <= grid[i][j] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    private fun help(a: Array<IntArray>, i: Int, j: Int, n: Int, m: Int, dp: Array<IntArray>): Int {
        if (i < 0 || i >= n || j >= m || j < 0) {
            return 0
        }
        if (dp[i][j] != 0) {
            return dp[i][j]
        }
        var res: Long = 0
        if (i < n - 1 && a[i + 1][j] > a[i][j]) {
            res += (1 + help(a, i + 1, j, n, m, dp)).toLong()
        }
        if (i > 0 && a[i - 1][j] > a[i][j]) {
            res += (1 + help(a, i - 1, j, n, m, dp)).toLong()
        }
        if (j > 0 && a[i][j - 1] > a[i][j]) {
            res += (1 + help(a, i, j - 1, n, m, dp)).toLong()
        }
        if (j < m - 1 && a[i][j + 1] > a[i][j]) {
            res += (1 + help(a, i, j + 1, n, m, dp)).toLong()
        }
        dp[i][j] = res.toInt() % 1000000007
        return dp[i][j]
    }

    fun countPaths(grid: Array<IntArray>): Int {
        val n = grid.size
        val m = grid[0].size
        var ans = n.toLong() * m
        val dp = Array(n) { IntArray(m) }
        for (i in 0 until n) {
            for (j in 0 until m) {
                ans += (help(grid, i, j, n, m, dp) % 1000000007).toLong()
            }
        }
        ans %= 1000000007
        return ans.toInt()
    }
}
```