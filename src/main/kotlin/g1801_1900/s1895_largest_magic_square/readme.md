[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1895\. Largest Magic Square

Medium

A `k x k` **magic square** is a `k x k` grid filled with integers such that every row sum, every column sum, and both diagonal sums are **all equal**. The integers in the magic square **do not have to be distinct**. Every `1 x 1` grid is trivially a **magic square**.

Given an `m x n` integer `grid`, return _the **size** (i.e., the side length_ `k`_) of the **largest magic square** that can be found within this grid_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/29/magicsquare-grid.jpg)

**Input:** grid = \[\[7,1,4,5,6],[2,5,1,6,4],[1,5,4,3,2],[1,2,7,3,4]]

**Output:** 3

**Explanation:** The largest magic square has a size of 3.

Every row sum, column sum, and diagonal sum of this magic square is equal to 12.

- Row sums: 5+1+6 = 5+4+3 = 2+7+3 = 12

- Column sums: 5+5+2 = 1+4+7 = 6+3+3 = 12

- Diagonal sums: 5+4+3 = 6+4+2 = 12 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/29/magicsquare2-grid.jpg)

**Input:** grid = \[\[5,1,3,1],[9,3,3,1],[1,3,3,8]]

**Output:** 2 

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 50`
*   <code>1 <= grid[i][j] <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun largestMagicSquare(grid: Array<IntArray>): Int {
        val m = grid.size
        val n = grid[0].size
        val rows = Array(m) { IntArray(n + 1) }
        val cols = Array(m + 1) { IntArray(n) }
        for (i in 0 until m) {
            for (j in 0 until n) {
                // cumulative sum for each row
                rows[i][j + 1] = rows[i][j] + grid[i][j]
                // cumulative sum for each column
                cols[i + 1][j] = cols[i][j] + grid[i][j]
            }
        }
        // start with the biggest side possible
        for (side in Math.min(m, n) downTo 2) {
            // check every square
            for (i in 0..m - side) {
                for (j in 0..n - side) {
                    // checks if a square with top left [i, j] and side length is magic
                    if (magic(grid, rows, cols, i, j, side)) {
                        return side
                    }
                }
            }
        }
        return 1
    }

    private fun magic(
        grid: Array<IntArray>,
        rows: Array<IntArray>,
        cols: Array<IntArray>,
        r: Int,
        c: Int,
        side: Int,
    ): Boolean {
        val sum = rows[r][c + side] - rows[r][c]
        var d1 = 0
        var d2 = 0
        for (k in 0 until side) {
            d1 += grid[r + k][c + k]
            d2 += grid[r + side - 1 - k][c + k]
            // check each row and column
            if (rows[r + k][c + side] - rows[r + k][c] != sum ||
                cols[r + side][c + k] - cols[r][c + k] != sum
            ) {
                return false
            }
        }
        // checks both diagonals
        return d1 == sum && d2 == sum
    }
}
```