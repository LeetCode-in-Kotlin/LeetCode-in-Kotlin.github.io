[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2827\. Number of Beautiful Integers in the Range

Hard

You are given positive integers `low`, `high`, and `k`.

A number is **beautiful** if it meets both of the following conditions:

*   The count of even digits in the number is equal to the count of odd digits.
*   The number is divisible by `k`.

Return _the number of beautiful integers in the range_ `[low, high]`.

**Example 1:**

**Input:** low = 10, high = 20, k = 3

**Output:** 2

**Explanation:** There are 2 beautiful integers in the given range: [12,18]. 
- 12 is beautiful because it contains 1 odd digit and 1 even digit, and is divisible by k = 3. 
- 18 is beautiful because it contains 1 odd digit and 1 even digit, and is divisible by k = 3. Additionally we can see that: 
- 16 is not beautiful because it is not divisible by k = 3. 
- 15 is not beautiful because it does not contain equal counts even and odd digits. It can be shown that there are only 2 beautiful integers in the given range.

**Example 2:**

**Input:** low = 1, high = 10, k = 1

**Output:** 1

**Explanation:** There is 1 beautiful integer in the given range: [10]. 
- 10 is beautiful because it contains 1 odd digit and 1 even digit, and is divisible by k = 1. It can be shown that there is only 1 beautiful integer in the given range.

**Example 3:**

**Input:** low = 5, high = 5, k = 2

**Output:** 0

**Explanation:** There are 0 beautiful integers in the given range. 
- 5 is not beautiful because it is not divisible by k = 2 and it does not contain equal even and odd digits.

**Constraints:**

*   <code>0 < low <= high <= 10<sup>9</sup></code>
*   `0 < k <= 20`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    private lateinit var dp: Array<Array<Array<Array<IntArray>>>>
    private var maxLength = 0

    fun numberOfBeautifulIntegers(low: Int, high: Int, k: Int): Int {
        val num1 = low.toString()
        val num2 = high.toString()
        maxLength = max(num1.length.toDouble(), num2.length.toDouble()).toInt()
        dp = Array(4) { Array(maxLength) { Array(maxLength) { Array(maxLength) { IntArray(k) } } } }
        for (a in dp) {
            for (b in a) {
                for (c in b) {
                    for (d in c) {
                        d.fill(-1)
                    }
                }
            }
        }
        return dp(num1, num2, 0, 3, 0, 0, 0, 0, k)
    }

    private fun dp(
        low: String,
        high: String,
        i: Int,
        mode: Int,
        odd: Int,
        even: Int,
        num: Int,
        rem: Int,
        k: Int
    ): Int {
        if (i == maxLength) {
            return if (num % k == 0 && odd == even) 1 else 0
        }
        if (dp[mode][i][odd][even][rem] != -1) {
            return dp[mode][i][odd][even][rem]
        }
        var res = 0
        val lowLimit = mode % 2 == 1
        val highLimit = mode / 2 == 1
        var start = 0
        var end = 9
        if (lowLimit) {
            start = digitAt(low, i)
        }
        if (highLimit) {
            end = digitAt(high, i)
        }
        for (j in start..end) {
            var newMode = 0
            if (j == start && lowLimit) {
                newMode += 1
            }
            if (j == end && highLimit) {
                newMode += 2
            }
            var newEven = even
            if (num != 0 || j != 0) {
                newEven += if (j % 2 == 0) 1 else 0
            }
            val newOdd = odd + (if (j % 2 == 1) 1 else 0)
            res +=
                dp(
                    low,
                    high,
                    i + 1,
                    newMode,
                    newOdd,
                    newEven,
                    num * 10 + j,
                    (num * 10 + j) % k,
                    k
                )
        }
        dp[mode][i][odd][even][rem] = res
        return res
    }

    private fun digitAt(num: String, i: Int): Int {
        val index = num.length - maxLength + i
        return if (index < 0) 0 else num[index].code - '0'.code
    }
}
```