[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3966\. Count Good Integers in a Range

Hard

You are given three integers `l`, `r` and `k`.

A number is considered **good** if the **absolute difference** between every pair of **adjacent** digits is **at most** `k`.

Return the number of **good** integers in the range `[l, r]` (inclusive).

The **absolute difference** between values `x` and `y` is defined as `abs(x - y)`.

**Example 1:**

**Input:** l = 10, r = 15, k = 1

**Output:** 3

**Explanation:**

*   The good integers in the range are 10, 11, and 12.
*   For 10, `abs(1 - 0) = 1`.
*   For 11, `abs(1 - 1) = 0`.
*   For 12, `abs(1 - 2) = 1`.
*   All these differences are at most `k = 1`. Thus, the answer is 3.

**Example 2:**

**Input:** l = 201, r = 204, k = 2

**Output:** 2

**Explanation:**

*   The good integers in the range are 201 and 202.
*   For 201, `abs(2 - 0) = 2` and `abs(0 - 1) = 1`.
*   For 202, `abs(2 - 0) = 2` and `abs(0 - 2) = 2`.
*   Thus, the answer is 2.

**Constraints:**

*   <code>10 <= l <= r <= 10<sup>15</sup></code>
*   `0 <= k <= 9`

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    private val dp: Array<Array<Array<Array<Long?>>>> = Array(16) {
        Array(2) {
            Array(10) {
                arrayOfNulls<Long>(10)
            }
        }
    }
    private val digits = IntArray(16)

    fun goodIntegers(l: Long, r: Long, k: Int): Long = calc(r, k) - calc(l - 1, k)

    private fun calc(num: Long, k: Int): Long {
        var temp = num
        var len = 0
        while (temp > 0) {
            digits[len++] = (temp % 10).toInt()
            temp /= 10
        }
        return dfs(len - 1, 0, 1, true, k)
    }

    private fun dfs(index: Int, pre: Int, isLeadingZero: Int, isMaxDigit: Boolean, k: Int): Long {
        if (index == -1) {
            return 1
        }
        if (!isMaxDigit && dp[index][isLeadingZero][pre][k] != null) {
            return dp[index][isLeadingZero][pre][k]!!
        }
        val maxDigit = if (isMaxDigit) digits[index] else 9
        var result = 0L
        for (d in 0..maxDigit) {
            if (isLeadingZero == 1 || abs(d - pre) <= k) {
                result += dfs(
                    index - 1,
                    d,
                    if (isLeadingZero == 1 && d == 0) 1 else 0,
                    isMaxDigit && d == maxDigit,
                    k,
                )
            }
        }
        if (!isMaxDigit) {
            dp[index][isLeadingZero][pre][k] = result
        }
        return result
    }
}
```