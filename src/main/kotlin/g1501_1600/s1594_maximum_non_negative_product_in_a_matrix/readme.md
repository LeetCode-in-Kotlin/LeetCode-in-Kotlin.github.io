[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1594\. Maximum Non Negative Product in a Matrix

Medium

You are given a `m x n` matrix `grid`. Initially, you are located at the top-left corner `(0, 0)`, and in each step, you can only **move right or down** in the matrix.

Among all possible paths starting from the top-left corner `(0, 0)` and ending in the bottom-right corner `(m - 1, n - 1)`, find the path with the **maximum non-negative product**. The product of a path is the product of all integers in the grid cells visited along the path.

Return the _maximum non-negative product **modulo**_ <code>10<sup>9</sup> + 7</code>. _If the maximum product is **negative**, return_ `-1`.

Notice that the modulo is performed after getting the maximum product.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/12/23/product1.jpg)

**Input:** grid = \[\[-1,-2,-3],[-2,-3,-3],[-3,-3,-2]]

**Output:** -1

**Explanation:** It is not possible to get non-negative product in the path from (0, 0) to (2, 2), so return -1.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/12/23/product2.jpg)

**Input:** grid = \[\[1,-2,1],[1,-2,1],[3,-4,1]]

**Output:** 8

**Explanation:** Maximum non-negative product is shown (1 \* 1 \* -2 \* -4 \* 1 = 8).

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/12/23/product3.jpg)

**Input:** grid = \[\[1,3],[0,-4]]

**Output:** 0

**Explanation:** Maximum non-negative product is shown (1 \* 0 \* -4 = 0).

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 15`
*   `-4 <= grid[i][j] <= 4`

## Solution

```kotlin
class Solution {
    private class Tuple(var max: Long, var min: Long)

    fun maxProductPath(grid: Array<IntArray?>?): Int {
        // DP
        if (grid == null || grid.size == 0 || grid[0] == null || grid[0]!!.size == 0) {
            return 0
        }
        val rows = grid.size
        val cols = grid[0]!!.size
        val dp = Array(rows) { arrayOfNulls<Tuple>(cols) }
        for (i in 0 until rows) {
            for (j in 0 until cols) {
                dp[i][j] = Tuple(1, 1)
            }
        }
        // Init first row and column
        dp[0][0]!!.max = grid[0]!![0].toLong()
        dp[0][0]!!.min = grid[0]!![0].toLong()
        for (i in 1 until rows) {
            dp[i][0]!!.max = grid[i]!![0] * dp[i - 1][0]!!.max
            dp[i][0]!!.min = grid[i]!![0] * dp[i - 1][0]!!.min
        }
        for (i in 1 until cols) {
            dp[0][i]!!.max = grid[0]!![i] * dp[0][i - 1]!!.max
            dp[0][i]!!.min = grid[0]!![i] * dp[0][i - 1]!!.min
        }
        // DP
        for (i in 1 until rows) {
            for (j in 1 until cols) {
                val up1 = dp[i - 1][j]!!.max * grid[i]!![j]
                val up2 = dp[i - 1][j]!!.min * grid[i]!![j]
                val left1 = dp[i][j - 1]!!.max * grid[i]!![j]
                val left2 = dp[i][j - 1]!!.min * grid[i]!![j]
                dp[i][j]!!.max = Math.max(up1, Math.max(up2, Math.max(left1, left2)))
                dp[i][j]!!.min = Math.min(up1, Math.min(up2, Math.min(left1, left2)))
            }
        }
        return if (dp[rows - 1][cols - 1]!!.max < 0) {
            -1
        } else (dp[rows - 1][cols - 1]!!.max % (1e9 + 7)).toInt()
    }
}
```