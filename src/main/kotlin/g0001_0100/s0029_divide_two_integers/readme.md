[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 29\. Divide Two Integers

Medium

Given two integers `dividend` and `divisor`, divide two integers **without** using multiplication, division, and mod operator.

The integer division should truncate toward zero, which means losing its fractional part. For example, `8.345` would be truncated to `8`, and `-2.7335` would be truncated to `-2`.

Return _the **quotient** after dividing_ `dividend` _by_ `divisor`.

**Note:** Assume we are dealing with an environment that could only store integers within the **32-bit** signed integer range: <code>[−2<sup>31</sup>, 2<sup>31</sup> − 1]</code>. For this problem, if the quotient is **strictly greater than** <code>2<sup>31</sup> - 1</code>, then return <code>2<sup>31</sup> - 1</code>, and if the quotient is **strictly less than** <code>-2<sup>31</sup></code>, then return <code>-2<sup>31</sup></code>.

**Example 1:**

**Input:** dividend = 10, divisor = 3

**Output:** 3

**Explanation:** 10/3 = 3.33333.. which is truncated to 3.

**Example 2:**

**Input:** dividend = 7, divisor = -3

**Output:** -2

**Explanation:** 7/-3 = -2.33333.. which is truncated to -2.

**Constraints:**

*   <code>-2<sup>31</sup> <= dividend, divisor <= 2<sup>31</sup> - 1</code>
*   `divisor != 0`

## Solution

```kotlin
@Suppress("INTEGER_OVERFLOW")
class Solution {
    fun divide(dividend: Int, divisor: Int): Int {
        val isNegative = dividend > 0 && divisor < 0 || dividend < 0 && divisor > 0
        var ans: Long = 0
        var divide = Math.abs(dividend.toLong())
        val divisorAbs = Math.abs(divisor.toLong())
        while (divide >= divisorAbs) {
            var temp = divisorAbs
            var cnt: Long = 1
            while (divide >= temp) {
                divide -= temp
                ans += cnt
                cnt = cnt shl 1
                temp = temp shl 1
            }
        }
        if (isNegative) {
            ans = -ans
        }
        val intMin = -(1 shl 31)
        val intMax = (1 shl 31) - 1
        if (ans < intMin || ans > intMax) {
            ans = intMax.toLong()
        }
        return ans.toInt()
    }
}
```