[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3393\. Count Paths With the Given XOR Value

Medium

You are given a 2D integer array `grid` with size `m x n`. You are also given an integer `k`.

Your task is to calculate the number of paths you can take from the top-left cell `(0, 0)` to the bottom-right cell `(m - 1, n - 1)` satisfying the following **constraints**:

*   You can either move to the right or down. Formally, from the cell `(i, j)` you may move to the cell `(i, j + 1)` or to the cell `(i + 1, j)` if the target cell _exists_.
*   The `XOR` of all the numbers on the path must be **equal** to `k`.

Return the total number of such paths.

Since the answer can be very large, return the result **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** grid = \[\[2, 1, 5], [7, 10, 0], [12, 6, 4]], k = 11

**Output:** 3

**Explanation:**

The 3 paths are:

*   `(0, 0) → (1, 0) → (2, 0) → (2, 1) → (2, 2)`
*   `(0, 0) → (1, 0) → (1, 1) → (1, 2) → (2, 2)`
*   `(0, 0) → (0, 1) → (1, 1) → (2, 1) → (2, 2)`

**Example 2:**

**Input:** grid = \[\[1, 3, 3, 3], [0, 3, 3, 2], [3, 0, 1, 1]], k = 2

**Output:** 5

**Explanation:**

The 5 paths are:

*   `(0, 0) → (1, 0) → (2, 0) → (2, 1) → (2, 2) → (2, 3)`
*   `(0, 0) → (1, 0) → (1, 1) → (2, 1) → (2, 2) → (2, 3)`
*   `(0, 0) → (1, 0) → (1, 1) → (1, 2) → (1, 3) → (2, 3)`
*   `(0, 0) → (0, 1) → (1, 1) → (1, 2) → (2, 2) → (2, 3)`
*   `(0, 0) → (0, 1) → (0, 2) → (1, 2) → (2, 2) → (2, 3)`

**Example 3:**

**Input:** grid = \[\[1, 1, 1, 2], [3, 0, 3, 2], [3, 0, 2, 2]], k = 10

**Output:** 0

**Constraints:**

*   `1 <= m == grid.length <= 300`
*   `1 <= n == grid[r].length <= 300`
*   `0 <= grid[r][c] < 16`
*   `0 <= k < 16`

## Solution

```kotlin
class Solution {
    private var m = -1
    private var n = -1
    private lateinit var dp: Array<Array<IntArray>>

    fun countPathsWithXorValue(grid: Array<IntArray>, k: Int): Int {
        m = grid.size
        n = grid[0].size
        dp = Array(m) { Array(n) { IntArray(16) { -1 } } }
        return dfs(grid, 0, k, 0, 0)
    }

    private fun dfs(grid: Array<IntArray>, xorVal: Int, k: Int, i: Int, j: Int): Int {
        var xorVal = xorVal
        if (i < 0 || j < 0 || j >= n || i >= m) {
            return 0
        }
        xorVal = xorVal xor grid[i][j]
        if (dp[i][j][xorVal] != -1) {
            return dp[i][j][xorVal]
        }
        if (i == m - 1 && j == n - 1 && xorVal == k) {
            return 1
        }
        val down = dfs(grid, xorVal, k, i + 1, j)
        val right = dfs(grid, xorVal, k, i, j + 1)
        dp[i][j][xorVal] = (down + right) % MOD
        return dp[i][j][xorVal]
    }

    companion object {
        private val MOD = (1e9 + 7).toInt()
    }
}
```