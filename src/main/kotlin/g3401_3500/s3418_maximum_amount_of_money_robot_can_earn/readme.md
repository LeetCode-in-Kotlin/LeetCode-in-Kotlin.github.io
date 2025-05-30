[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3418\. Maximum Amount of Money Robot Can Earn

Medium

You are given an `m x n` grid. A robot starts at the top-left corner of the grid `(0, 0)` and wants to reach the bottom-right corner `(m - 1, n - 1)`. The robot can move either right or down at any point in time.

The grid contains a value `coins[i][j]` in each cell:

*   If `coins[i][j] >= 0`, the robot gains that many coins.
*   If `coins[i][j] < 0`, the robot encounters a robber, and the robber steals the **absolute** value of `coins[i][j]` coins.

The robot has a special ability to **neutralize robbers** in at most **2 cells** on its path, preventing them from stealing coins in those cells.

**Note:** The robot's total coins can be negative.

Return the **maximum** profit the robot can gain on the route.

**Example 1:**

**Input:** coins = \[\[0,1,-1],[1,-2,3],[2,-3,4]]

**Output:** 8

**Explanation:**

An optimal path for maximum coins is:

1.  Start at `(0, 0)` with `0` coins (total coins = `0`).
2.  Move to `(0, 1)`, gaining `1` coin (total coins = `0 + 1 = 1`).
3.  Move to `(1, 1)`, where there's a robber stealing `2` coins. The robot uses one neutralization here, avoiding the robbery (total coins = `1`).
4.  Move to `(1, 2)`, gaining `3` coins (total coins = `1 + 3 = 4`).
5.  Move to `(2, 2)`, gaining `4` coins (total coins = `4 + 4 = 8`).

**Example 2:**

**Input:** coins = \[\[10,10,10],[10,10,10]]

**Output:** 40

**Explanation:**

An optimal path for maximum coins is:

1.  Start at `(0, 0)` with `10` coins (total coins = `10`).
2.  Move to `(0, 1)`, gaining `10` coins (total coins = `10 + 10 = 20`).
3.  Move to `(0, 2)`, gaining another `10` coins (total coins = `20 + 10 = 30`).
4.  Move to `(1, 2)`, gaining the final `10` coins (total coins = `30 + 10 = 40`).

**Constraints:**

*   `m == coins.length`
*   `n == coins[i].length`
*   `1 <= m, n <= 500`
*   `-1000 <= coins[i][j] <= 1000`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximumAmount(coins: Array<IntArray>): Int {
        val m = coins.size
        val n = coins[0].size
        val dp = Array<IntArray>(m) { IntArray(n) }
        val dp1 = Array<IntArray>(m) { IntArray(n) }
        val dp2 = Array<IntArray>(m) { IntArray(n) }
        dp[0][0] = coins[0][0]
        for (j in 1..<n) {
            dp[0][j] = dp[0][j - 1] + coins[0][j]
        }
        for (i in 1..<m) {
            dp[i][0] = dp[i - 1][0] + coins[i][0]
        }
        for (i in 1..<m) {
            for (j in 1..<n) {
                dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]) + coins[i][j]
            }
        }
        dp1[0][0] = max(coins[0][0], 0)
        for (j in 1..<n) {
            dp1[0][j] = max(dp[0][j - 1], (dp1[0][j - 1] + coins[0][j]))
        }
        for (i in 1..<m) {
            dp1[i][0] = max(dp[i - 1][0], (dp1[i - 1][0] + coins[i][0]))
        }
        for (i in 1..<m) {
            for (j in 1..<n) {
                dp1[i][j] = max(
                    max(dp[i][j - 1], dp[i - 1][j]),
                    (max(dp1[i][j - 1], dp1[i - 1][j]) + coins[i][j]),
                )
            }
        }
        dp2[0][0] = max(coins[0][0], 0)
        for (j in 1..<n) {
            dp2[0][j] = max(dp1[0][j - 1], (dp2[0][j - 1] + coins[0][j]))
        }
        for (i in 1..<m) {
            dp2[i][0] = max(dp1[i - 1][0], (dp2[i - 1][0] + coins[i][0]))
        }
        for (i in 1..<m) {
            for (j in 1..<n) {
                dp2[i][j] = max(
                    max(dp1[i][j - 1], dp1[i - 1][j]),
                    (max(dp2[i][j - 1], dp2[i - 1][j]) + coins[i][j]),
                )
            }
        }
        return dp2[m - 1][n - 1]
    }
}
```