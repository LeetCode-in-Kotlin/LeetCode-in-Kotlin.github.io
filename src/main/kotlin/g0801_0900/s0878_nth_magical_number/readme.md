[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 878\. Nth Magical Number

Hard

A positive integer is _magical_ if it is divisible by either `a` or `b`.

Given the three integers `n`, `a`, and `b`, return the <code>n<sup>th</sup></code> magical number. Since the answer may be very large, **return it modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 1, a = 2, b = 3

**Output:** 2

**Example 2:**

**Input:** n = 4, a = 2, b = 3

**Output:** 6

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>
*   <code>2 <= a, b <= 4 * 10<sup>4</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun nthMagicalNumber(n: Int, a: Int, b: Int): Int {
        val c = lcm(a.toLong(), b.toLong())
        var l: Long = 2
        var r = n * c
        var ans = r
        while (l <= r) {
            val mid = l + (r - l) / 2
            val cnt = mid / a + mid / b - mid / c
            if (cnt < n) {
                l = mid + 1
            } else {
                ans = mid
                r = mid - 1
            }
        }
        return (ans % MOD).toInt()
    }

    private fun lcm(a: Long, b: Long): Long {
        return a * b / gcd(a, b)
    }

    private fun gcd(a: Long, b: Long): Long {
        var a = a
        var b = b
        var t: Long
        while (b != 0L) {
            t = b
            b = a % b
            a = t
        }
        return a
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```