[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2801\. Count Stepping Numbers in Range

Hard

Given two positive integers `low` and `high` represented as strings, find the count of **stepping numbers** in the inclusive range `[low, high]`.

A **stepping number** is an integer such that all of its adjacent digits have an absolute difference of **exactly** `1`.

Return _an integer denoting the count of stepping numbers in the inclusive range_ `[low, high]`_._

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Note:** A stepping number should not have a leading zero.

**Example 1:**

**Input:** low = "1", high = "11"

**Output:** 10

**Explanation:** The stepping numbers in the range [1,11] are 1, 2, 3, 4, 5, 6, 7, 8, 9 and 10. There are a total of 10 stepping numbers in the range. Hence, the output is 10.

**Example 2:**

**Input:** low = "90", high = "101"

**Output:** 2

**Explanation:** The stepping numbers in the range [90,101] are 98 and 101. There are a total of 2 stepping numbers in the range. Hence, the output is 2.

**Constraints:**

*   <code>1 <= int(low) <= int(high) < 10<sup>100</sup></code>
*   `1 <= low.length, high.length <= 100`
*   `low` and `high` consist of only digits.
*   `low` and `high` don't have any leading zeros.

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    private lateinit var dp: Array<Array<Array<Array<Int?>>>>

    fun countSteppingNumbers(low: String, high: String): Int {
        dp = Array(low.length + 1) { Array(10) { Array(2) { arrayOfNulls(2) } } }
        val count1 = solve(low, 0, 0, 1, 1)
        dp = Array(high.length + 1) { Array(10) { Array(2) { arrayOfNulls(2) } } }
        val count2 = solve(high, 0, 0, 1, 1)
        return (count2!! - count1!! + isStep(low) + MOD) % MOD
    }

    private fun solve(s: String, i: Int, prevDigit: Int, hasBound: Int, curIsZero: Int): Int? {
        if (i >= s.length) {
            if (curIsZero == 1) {
                return 0
            }
            return 1
        }
        if (dp[i][prevDigit][hasBound][curIsZero] != null) {
            return dp[i][prevDigit][hasBound][curIsZero]
        }
        var count = 0
        var limit = 9
        if (hasBound == 1) {
            limit = s[i].code - '0'.code
        }
        for (digit in 0..limit) {
            val nextIsZero = if ((curIsZero == 1 && digit == 0)) 1 else 0
            val nextHasBound = if ((hasBound == 1 && digit == limit)) 1 else 0
            if (curIsZero == 1 || abs(digit - prevDigit) == 1) {
                count = (count + solve(s, i + 1, digit, nextHasBound, nextIsZero)!!) % MOD
            }
        }
        dp[i][prevDigit][hasBound][curIsZero] = count
        return dp[i][prevDigit][hasBound][curIsZero]
    }

    private fun isStep(s: String): Int {
        var isValid = true
        for (i in 0 until s.length - 1) {
            if (abs((s[i + 1].code - s[i].code)) != 1) {
                isValid = false
                break
            }
        }
        if (isValid) {
            return 1
        }
        return 0
    }

    companion object {
        private const val MOD = (1e9 + 7).toInt()
    }
}
```