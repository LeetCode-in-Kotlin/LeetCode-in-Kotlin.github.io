[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 329\. Longest Increasing Path in a Matrix

Hard

Given an `m x n` integers `matrix`, return _the length of the longest increasing path in_ `matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You **may not** move **diagonally** or move **outside the boundary** (i.e., wrap-around is not allowed).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg)

**Input:** matrix = \[\[9,9,4],[6,6,8],[2,1,1]]

**Output:** 4

**Explanation:** The longest increasing path is `[1, 2, 6, 9]`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/27/tmp-grid.jpg)

**Input:** matrix = \[\[3,4,5],[3,2,6],[2,2,1]]

**Output:** 4

**Explanation:** The longest increasing path is `[3, 4, 5, 6]`. Moving diagonally is not allowed.

**Example 3:**

**Input:** matrix = \[\[1]]

**Output:** 1

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 200`
*   <code>0 <= matrix[i][j] <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    fun longestIncreasingPath(matrix: Array<IntArray>): Int {
        var maxIncreasingSequenceCount = 0
        val n = matrix.size - 1
        val m = matrix[0].size - 1
        val memory = Array(n + 1) { IntArray(m + 1) }
        for (i in matrix.indices) {
            for (j in matrix[i].indices) {
                maxIncreasingSequenceCount = Math.max(maxIncreasingSequenceCount, move(i, j, n, m, matrix, memory))
            }
        }
        return maxIncreasingSequenceCount
    }

    private fun move(row: Int, col: Int, n: Int, m: Int, matrix: Array<IntArray>, memory: Array<IntArray>): Int {
        if (memory[row][col] == 0) {
            var count = 1
            // move down
            if (row < n && matrix[row + 1][col] > matrix[row][col]) {
                count = Math.max(count, move(row + 1, col, n, m, matrix, memory) + 1)
            }
            // move right
            if (col < m && matrix[row][col + 1] > matrix[row][col]) {
                count = Math.max(count, move(row, col + 1, n, m, matrix, memory) + 1)
            }
            // move up
            if (row > 0 && matrix[row - 1][col] > matrix[row][col]) {
                count = Math.max(count, move(row - 1, col, n, m, matrix, memory) + 1)
            }
            // move left
            if (col > 0 && matrix[row][col - 1] > matrix[row][col]) {
                count = Math.max(count, move(row, col - 1, n, m, matrix, memory) + 1)
            }
            memory[row][col] = count
        }
        return memory[row][col]
    }
}
```