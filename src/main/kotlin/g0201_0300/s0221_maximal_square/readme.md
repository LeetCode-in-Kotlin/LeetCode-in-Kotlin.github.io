[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 221\. Maximal Square

Medium

Given an `m x n` binary `matrix` filled with `0`'s and `1`'s, _find the largest square containing only_ `1`'s _and return its area_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)

**Input:** matrix = \[\["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]

**Output:** 4

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg)

**Input:** matrix = \[\["0","1"],["1","0"]]

**Output:** 1

**Example 3:**

**Input:** matrix = \[\["0"]]

**Output:** 0

**Constraints:**

*   `m == matrix.length`
*   `n == matrix[i].length`
*   `1 <= m, n <= 300`
*   `matrix[i][j]` is `'0'` or `'1'`.

## Solution

```kotlin
class Solution {
    fun maximalSquare(matrix: Array<CharArray>): Int {
        val m = matrix.size
        if (m == 0) {
            return 0
        }
        val n: Int = matrix[0].size
        if (n == 0) {
            return 0
        }
        val dp = Array(m + 1) { IntArray(n + 1) }
        var max = 0
        for (i in 0 until m) {
            for (j in 0 until n) {
                if (matrix[i][j] == '1') {
                    // 1 + minimum from cell above, cell to the left, cell diagonal upper-left
                    val next = 1 + Math.min(dp[i][j], Math.min(dp[i + 1][j], dp[i][j + 1]))
                    // keep track of the maximum value seen
                    if (next > max) {
                        max = next
                    }
                    dp[i + 1][j + 1] = next
                }
            }
        }
        return max * max
    }
}
```