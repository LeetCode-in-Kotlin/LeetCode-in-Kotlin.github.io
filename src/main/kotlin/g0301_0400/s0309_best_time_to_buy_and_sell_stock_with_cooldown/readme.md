[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 309\. Best Time to Buy and Sell Stock with Cooldown

Medium

You are given an array `prices` where `prices[i]` is the price of a given stock on the <code>i<sup>th</sup></code> day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

*   After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** prices = [1,2,3,0,2]

**Output:** 3

**Explanation:** transactions = [buy, sell, cooldown, buy, sell]

**Example 2:**

**Input:** prices = [1]

**Output:** 0

**Constraints:**

*   `1 <= prices.length <= 5000`
*   `0 <= prices[i] <= 1000`

## Solution

```kotlin
class Solution {
    fun maxProfit(prices: IntArray): Int {
        var sell = 0
        var prevSell = 0
        var buy = Int.MIN_VALUE
        var prevBuy: Int
        for (price in prices) {
            prevBuy = buy
            buy = Math.max(prevSell - price, prevBuy)
            prevSell = sell
            sell = Math.max(prevBuy + price, prevSell)
        }
        return sell
    }
}
```