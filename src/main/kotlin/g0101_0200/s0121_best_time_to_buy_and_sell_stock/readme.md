[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 121\. Best Time to Buy and Sell Stock

Easy

You are given an array `prices` where `prices[i]` is the price of a given stock on the <code>i<sup>th</sup></code> day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.

**Example 1:**

**Input:** prices = [7,1,5,3,6,4]

**Output:** 5

**Explanation:** Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5. 

Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

**Example 2:**

**Input:** prices = [7,6,4,3,1]

**Output:** 0

**Explanation:** In this case, no transactions are done and the max profit = 0.

**Constraints:**

*   <code>1 <= prices.length <= 10<sup>5</sup></code>
*   <code>0 <= prices[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun maxProfit(prices: IntArray): Int {
        var maxProfit = 0
        var min = prices[0]
        for (i in 1 until prices.size) {
            if (prices[i] > min) {
                maxProfit = Math.max(maxProfit, prices[i] - min)
            } else {
                min = prices[i]
            }
        }
        return maxProfit
    }
}
```