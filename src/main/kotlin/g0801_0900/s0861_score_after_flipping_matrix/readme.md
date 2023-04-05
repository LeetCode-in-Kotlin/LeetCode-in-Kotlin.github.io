[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 861\. Score After Flipping Matrix

Medium

You are given an `m x n` binary matrix `grid`.

A **move** consists of choosing any row or column and toggling each value in that row or column (i.e., changing all `0`'s to `1`'s, and all `1`'s to `0`'s).

Every row of the matrix is interpreted as a binary number, and the **score** of the matrix is the sum of these numbers.

Return _the highest possible **score** after making any number of **moves** (including zero moves)_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-toogle1.jpg)

**Input:** grid = \[\[0,0,1,1],[1,0,1,0],[1,1,0,0]]

**Output:** 39

**Explanation:** 0b1111 + 0b1001 + 0b1111 = 15 + 9 + 15 = 39

**Example 2:**

**Input:** grid = \[\[0]]

**Output:** 1

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 20`
*   `grid[i][j]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    fun matrixScore(grid: Array<IntArray>): Int {
        val m = grid.size
        val n = grid[0].size
        var score = 0

        // Flip rows to make the first column all 1's
        for (i in 0 until m) {
            if (grid[i][0] == 0) {
                flipRow(grid, i)
            }
        }

        // Flip columns to maximize the score
        for (j in 1 until n) {
            var ones = 0
            for (i in 0 until m) {
                ones += grid[i][j]
            }
            if (ones < m - ones) {
                flipColumn(grid, j)
            }
        }

        // Calculate the score
        for (i in 0 until m) {
            var rowScore = 0
            for (j in 0 until n) {
                rowScore = rowScore * 2 + grid[i][j]
            }
            score += rowScore
        }
        return score
    }

    private fun flipRow(grid: Array<IntArray>, i: Int) {
        val n = grid[0].size
        for (j in 0 until n) {
            grid[i][j] = 1 - grid[i][j]
        }
    }

    private fun flipColumn(grid: Array<IntArray>, j: Int) {
        val m = grid.size
        for (i in 0 until m) {
            grid[i][j] = 1 - grid[i][j]
        }
    }
}
```