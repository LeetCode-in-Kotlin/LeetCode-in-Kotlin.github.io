[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3240\. Minimum Number of Flips to Make Binary Grid Palindromic II

Medium

You are given an `m x n` binary matrix `grid`.

A row or column is considered **palindromic** if its values read the same forward and backward.

You can **flip** any number of cells in `grid` from `0` to `1`, or from `1` to `0`.

Return the **minimum** number of cells that need to be flipped to make **all** rows and columns **palindromic**, and the total number of `1`'s in `grid` **divisible** by `4`.

**Example 1:**

**Input:** grid = \[\[1,0,0],[0,1,0],[0,0,1]]

**Output:** 3

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/08/01/image.png)

**Example 2:**

**Input:** grid = \[\[0,1],[0,1],[0,0]]

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/07/08/screenshot-from-2024-07-09-01-37-48.png)

**Example 3:**

**Input:** grid = \[\[1],[1]]

**Output:** 2

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/08/01/screenshot-from-2024-08-01-23-05-26.png)

**Constraints:**

*   `m == grid.length`
*   `n == grid[i].length`
*   <code>1 <= m * n <= 2 * 10<sup>5</sup></code>
*   `0 <= grid[i][j] <= 1`

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minFlips(grid: Array<IntArray>): Int {
        var res = 0
        var one = 0
        var diff = 0
        val m = grid.size
        val n = grid[0].size
        // Handle quadrants
        for (i in 0 until m / 2) {
            for (j in 0 until n / 2) {
                val v =
                    (
                        grid[i][j] +
                            grid[i][n - 1 - j] +
                            grid[m - 1 - i][j] +
                            grid[m - 1 - i][n - 1 - j]
                        )
                res += min(v, (4 - v))
            }
        }
        // Handle middle column
        if (n % 2 > 0) {
            for (i in 0 until m / 2) {
                diff += grid[i][n / 2] xor grid[m - 1 - i][n / 2]
                one += grid[i][n / 2] + grid[m - 1 - i][n / 2]
            }
        }
        // Handle middle row
        if (m % 2 > 0) {
            for (j in 0 until n / 2) {
                diff += grid[m / 2][j] xor grid[m / 2][n - 1 - j]
                one += grid[m / 2][j] + grid[m / 2][n - 1 - j]
            }
        }
        // Handle center point
        if (n % 2 > 0 && m % 2 > 0) {
            res += grid[m / 2][n / 2]
        }
        // Divisible by 4
        if (diff == 0 && one % 4 > 0) {
            res += 2
        }
        return res + diff
    }
}
```