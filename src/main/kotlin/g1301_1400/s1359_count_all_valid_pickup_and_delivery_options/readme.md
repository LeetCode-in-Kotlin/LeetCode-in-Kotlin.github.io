[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1359\. Count All Valid Pickup and Delivery Options

Hard

Given `n` orders, each order consist in pickup and delivery services.

Count all valid pickup/delivery possible sequences such that delivery(i) is always after of pickup(i).

Since the answer may be too large, return it modulo 10^9 + 7.

**Example 1:**

**Input:** n = 1

**Output:** 1

**Explanation:** Unique order (P1, D1), Delivery 1 always is after of Pickup 1.

**Example 2:**

**Input:** n = 2

**Output:** 6

**Explanation:** All possible orders: 

(P1,P2,D1,D2), (P1,P2,D2,D1), (P1,D1,P2,D2), (P2,P1,D1,D2), (P2,P1,D2,D1) and (P2,D2,P1,D1). 

This is an invalid order (P1,D2,P2,D1) because Pickup 2 is after of Delivery 2.

**Example 3:**

**Input:** n = 3

**Output:** 90

**Constraints:**

*   `1 <= n <= 500`

## Solution

```kotlin
class Solution {
    fun countOrders(n: Int): Int {
        val dp = LongArray(n + 1)
        dp[1] = 1
        val mod = 1e9.toLong() + 7
        for (i in 2..n) {
            val gaps = (i - 1) * 2L + 1
            dp[i] = gaps * (gaps + 1) / 2 * dp[i - 1] % mod
        }
        return dp[n].toInt()
    }
}
```