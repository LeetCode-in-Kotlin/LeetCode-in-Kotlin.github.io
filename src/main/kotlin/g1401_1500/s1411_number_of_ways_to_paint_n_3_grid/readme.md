[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1411\. Number of Ways to Paint N × 3 Grid

Hard

You have a `grid` of size `n x 3` and you want to paint each cell of the grid with exactly one of the three colors: **Red**, **Yellow,** or **Green** while making sure that no two adjacent cells have the same color (i.e., no two cells that share vertical or horizontal sides have the same color).

Given `n` the number of rows of the grid, return _the number of ways_ you can paint this `grid`. As the answer may grow large, the answer **must be** computed modulo <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/03/26/e1.png)

**Input:** n = 1

**Output:** 12

**Explanation:** There are 12 possible way to paint the grid as shown.

**Example 2:**

**Input:** n = 5000

**Output:** 30228214

**Constraints:**

*   `n == grid.length`
*   `1 <= n <= 5000`

## Solution

```kotlin
class Solution {
    fun numOfWays(n: Int): Int {
        val dp = Array(n + 1) { IntArray(12) }
        dp[1].fill(1)
        val transfer = arrayOf(
            intArrayOf(5, 6, 8, 9, 10), intArrayOf(5, 8, 7, 9, -1),
            intArrayOf(5, 6, 9, 10, 12), intArrayOf(6, 10, 11, 12, -1), intArrayOf(1, 2, 3, 11, 12),
            intArrayOf(1, 3, 4, 11, -1), intArrayOf(2, 9, 10, 12, -1), intArrayOf(1, 2, 10, 11, 12),
            intArrayOf(1, 2, 3, 7, -1), intArrayOf(1, 3, 4, 7, 8), intArrayOf(4, 5, 6, 8, -1),
            intArrayOf(3, 4, 5, 7, 8)
        )
        for (i in 2..n) {
            for (j in 0..11) {
                val prevStates = transfer[j]
                var sum = 0
                for (s in prevStates) {
                    if (s == -1) {
                        break
                    }
                    sum = (sum + dp[i - 1][s - 1]) % MOD
                }
                dp[i][j] = sum
            }
        }
        var total = 0
        for (i in 0..11) {
            total = (total + dp[n][i]) % MOD
        }
        return total
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```