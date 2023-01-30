[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 576\. Out of Boundary Paths

Medium

There is an `m x n` grid with a ball. The ball is initially at the position `[startRow, startColumn]`. You are allowed to move the ball to one of the four adjacent cells in the grid (possibly out of the grid crossing the grid boundary). You can apply **at most** `maxMove` moves to the ball.

Given the five integers `m`, `n`, `maxMove`, `startRow`, `startColumn`, return the number of paths to move the ball out of the grid boundary. Since the answer can be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_1.png)

**Input:** m = 2, n = 2, maxMove = 2, startRow = 0, startColumn = 0

**Output:** 6

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_2.png)

**Input:** m = 1, n = 3, maxMove = 3, startRow = 0, startColumn = 1

**Output:** 12

**Constraints:**

*   `1 <= m, n <= 50`
*   `0 <= maxMove <= 50`
*   `0 <= startRow < m`
*   `0 <= startColumn < n`

## Solution

```kotlin
import java.util.Arrays

class Solution {
    private val dRowCol = arrayOf(intArrayOf(1, 0), intArrayOf(-1, 0), intArrayOf(0, 1), intArrayOf(0, -1))
    private fun dfs(
        m: Int,
        n: Int,
        remainingMoves: Int,
        currRow: Int,
        currCol: Int,
        cache: Array<Array<IntArray>>
    ): Int {
        if (currRow < 0 || currRow == m || currCol < 0 || currCol == n) {
            return 1
        }
        if (remainingMoves == 0) {
            return 0
        }
        if (cache[currRow][currCol][remainingMoves] == -1) {
            var paths = 0
            for (i in 0..3) {
                val newRow = currRow + dRowCol[i][0]
                val newCol = currCol + dRowCol[i][1]
                val m1 = 1000000007
                paths = (paths + dfs(m, n, remainingMoves - 1, newRow, newCol, cache)) % m1
            }
            cache[currRow][currCol][remainingMoves] = paths
        }
        return cache[currRow][currCol][remainingMoves]
    }

    fun findPaths(m: Int, n: Int, maxMoves: Int, startRow: Int, startCol: Int): Int {
        val cache = Array(m) {
            Array(n) {
                IntArray(
                    maxMoves + 1
                )
            }
        }
        for (c1 in cache) {
            for (c2 in c1) {
                Arrays.fill(c2, -1)
            }
        }
        return dfs(m, n, maxMoves, startRow, startCol, cache)
    }
}
```