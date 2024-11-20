[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1444\. Number of Ways of Cutting a Pizza

Hard

Given a rectangular pizza represented as a `rows x cols` matrix containing the following characters: `'A'` (an apple) and `'.'` (empty cell) and given the integer `k`. You have to cut the pizza into `k` pieces using `k-1` cuts.

For each cut you choose the direction: vertical or horizontal, then you choose a cut position at the cell boundary and cut the pizza into two pieces. If you cut the pizza vertically, give the left part of the pizza to a person. If you cut the pizza horizontally, give the upper part of the pizza to a person. Give the last piece of pizza to the last person.

_Return the number of ways of cutting the pizza such that each piece contains **at least** one apple. _Since the answer can be a huge number, return this modulo 10^9 + 7.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2020/04/23/ways_to_cut_apple_1.png)**

**Input:** pizza = ["A..","AAA","..."], k = 3

**Output:** 3

**Explanation:** The figure above shows the three ways to cut the pizza. Note that pieces must contain at least one apple.

**Example 2:**

**Input:** pizza = ["A..","AA.","..."], k = 3

**Output:** 1

**Example 3:**

**Input:** pizza = ["A..","A..","..."], k = 1

**Output:** 1

**Constraints:**

*   `1 <= rows, cols <= 50`
*   `rows == pizza.length`
*   `cols == pizza[i].length`
*   `1 <= k <= 10`
*   `pizza` consists of characters `'A'` and `'.'` only.

## Solution

```kotlin
class Solution {
    fun ways(pizza: Array<String>, k: Int): Int {
        if (pizza.isEmpty()) {
            return 0
        }
        val m = pizza.size
        val n = pizza[0].length
        val prefix = Array(m + 1) { IntArray(n + 1) }
        for (i in 0 until m) {
            for (j in 0 until n) {
                prefix[i + 1][j + 1] = (
                    (
                        prefix[i][j + 1] +
                            prefix[i + 1][j] +
                            if (pizza[i][j] == 'A') 1 else 0
                        ) -
                        prefix[i][j]
                    )
            }
        }
        val dp = Array(m) { Array(n) { IntArray(k) } }
        for (i in 0 until m) {
            for (j in 0 until n) {
                for (s in 0 until k) {
                    dp[i][j][s] = -1
                }
            }
        }
        return dfs(0, 0, m, n, k - 1, prefix, dp)
    }

    private fun dfs(
        m: Int,
        n: Int,
        temp1: Int,
        temp2: Int,
        k: Int,
        prefix: Array<IntArray>,
        dp: Array<Array<IntArray>>,
    ): Int {
        if (k == 0) {
            return if (hasApple(prefix, m, n, temp1 - 1, temp2 - 1)) 1 else 0
        }
        if (dp[m][n][k] != -1) {
            return dp[m][n][k]
        }
        var local = 0
        for (x in m until temp1 - 1) {
            local = (
                (
                    local +
                        (if (hasApple(prefix, m, n, x, temp2 - 1)) 1 else 0) *
                        dfs(x + 1, n, temp1, temp2, k - 1, prefix, dp)
                    ) %
                    K_MOD
                )
        }
        for (y in n until temp2 - 1) {
            local = (
                (
                    local +
                        (if (hasApple(prefix, m, n, temp1 - 1, y)) 1 else 0) *
                        dfs(m, y + 1, temp1, temp2, k - 1, prefix, dp)
                    ) %
                    K_MOD
                )
        }
        dp[m][n][k] = local
        return dp[m][n][k]
    }

    private fun hasApple(prefix: Array<IntArray>, x1: Int, y1: Int, x2: Int, y2: Int): Boolean {
        return (
            prefix[x2 + 1][y2 + 1] - prefix[x1][y2 + 1] - prefix[x2 + 1][y1] + prefix[x1][y1]
                > 0
            )
    }

    companion object {
        private const val K_MOD = (1e9 + 7).toInt()
    }
}
```