[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 790\. Domino and Tromino Tiling

Medium

You have two types of tiles: a `2 x 1` domino shape and a tromino shape. You may rotate these shapes.

![](https://assets.leetcode.com/uploads/2021/07/15/lc-domino.jpg)

Given an integer n, return _the number of ways to tile an_ `2 x n` _board_. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

In a tiling, every square must be covered by a tile. Two tilings are different if and only if there are two 4-directionally adjacent cells on the board such that exactly one of the tilings has both squares occupied by a tile.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/15/lc-domino1.jpg)

**Input:** n = 3

**Output:** 5

**Explanation:** The five different ways are show above.

**Example 2:**

**Input:** n = 1

**Output:** 1

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```kotlin
class Solution {
    fun numTilings(n: Int): Int {
        val result = when (n) {
            1 -> 1
            2 -> 2
            3 -> 5
            4 -> 11
            5 -> 24
            else -> {
                val dp = LongArray(n + 1)
                dp[0] = 0
                dp[1] = 1
                dp[2] = 2
                dp[3] = 5
                dp[4] = 11
                dp[5] = 24
                dp[6] = 53
                for (i in 7..n) {
                    dp[i] = (dp[i - 1] * 2 + dp[i - 3]) % 1000000007
                }
                return dp[n].toInt() % 1000000007
            }
        }
        return result
    }
}
```