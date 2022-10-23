[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 188\. Best Time to Buy and Sell Stock IV

Hard

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the <code>i<sup>th</sup></code> day, and an integer `k`.

Find the maximum profit you can achieve. You may complete at most `k` transactions.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** k = 2, prices = [2,4,1]

**Output:** 2

**Explanation:** Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.

**Example 2:**

**Input:** k = 2, prices = [3,2,6,5,0,3]

**Output:** 7

**Explanation:** Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.

**Constraints:**

*   `1 <= k <= 100`
*   `1 <= prices.length <= 1000`
*   `0 <= prices[i] <= 1000`

## Solution

```kotlin
class Solution {
    fun maxProfit(k: Int, prices: IntArray): Int {
        val n = prices.size
        val dp = IntArray(k + 1)
        val maxdp = IntArray(k + 1)
        for (i in 0..k) {
            maxdp[i] = Int.MIN_VALUE
        }
        for (i in 1..n) {
            maxdp[0] = Math.max(maxdp[0], dp[0] - prices[i - 1])
            for (j in k downTo 1) {
                maxdp[j] = Math.max(maxdp[j], dp[j] - prices[i - 1])
                dp[j] = Math.max(maxdp[j - 1] + prices[i - 1], dp[j])
            }
        }
        return dp[k]
    }
}
```