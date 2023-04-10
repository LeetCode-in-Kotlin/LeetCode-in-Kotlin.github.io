[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 879\. Profitable Schemes

Hard

There is a group of `n` members, and a list of various crimes they could commit. The <code>i<sup>th</sup></code> crime generates a `profit[i]` and requires `group[i]` members to participate in it. If a member participates in one crime, that member can't participate in another crime.

Let's call a **profitable scheme** any subset of these crimes that generates at least `minProfit` profit, and the total number of members participating in that subset of crimes is at most `n`.

Return the number of schemes that can be chosen. Since the answer may be very large, **return it modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 5, minProfit = 3, group = [2,2], profit = [2,3]

**Output:** 2

**Explanation:** To make a profit of at least 3, the group could either commit crimes 0 and 1, or just crime 1. In total, there are 2 schemes.

**Example 2:**

**Input:** n = 10, minProfit = 5, group = [2,3,5], profit = [6,7,8]

**Output:** 7

**Explanation:** To make a profit of at least 5, the group could commit any crimes, as long as they commit one. There are 7 possible schemes: (0), (1), (2), (0,1), (0,2), (1,2), and (0,1,2).

**Constraints:**

*   `1 <= n <= 100`
*   `0 <= minProfit <= 100`
*   `1 <= group.length <= 100`
*   `1 <= group[i] <= 100`
*   `profit.length == group.length`
*   `0 <= profit[i] <= 100`

## Solution

```kotlin
class Solution {
    fun profitableSchemes(n: Int, minProfit: Int, group: IntArray, profit: IntArray): Int {
        val dp = Array(n + 1) { LongArray(minProfit + 1) }
        val modulus = 1000000007L
        for (i in dp.indices) {
            dp[i][0] = 1
        }
        for (i in group.indices) {
            val currWorker = group[i]
            val currProfit = profit[i]
            for (j in dp.size - 1 downTo currWorker) {
                for (k in dp[j].indices.reversed()) {
                    dp[j][k] = (
                        (dp[j][k] + dp[j - currWorker][(k - currProfit).coerceAtLeast(0)]) %
                            modulus
                        )
                }
            }
        }
        return dp[n][minProfit].toInt()
    }
}
```