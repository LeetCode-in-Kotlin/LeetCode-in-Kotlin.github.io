[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1201\. Ugly Number III

Medium

An **ugly number** is a positive integer that is divisible by `a`, `b`, or `c`.

Given four integers `n`, `a`, `b`, and `c`, return the <code>n<sup>th</sup></code> **ugly number**.

**Example 1:**

**Input:** n = 3, a = 2, b = 3, c = 5

**Output:** 4

**Explanation:** The ugly numbers are 2, 3, 4, 5, 6, 8, 9, 10... The 3<sup>rd</sup> is 4.

**Example 2:**

**Input:** n = 4, a = 2, b = 3, c = 4

**Output:** 6

**Explanation:** The ugly numbers are 2, 3, 4, 6, 8, 9, 10, 12... The 4<sup>th</sup> is 6.

**Example 3:**

**Input:** n = 5, a = 2, b = 11, c = 13

**Output:** 10

**Explanation:** The ugly numbers are 2, 4, 6, 8, 10, 11, 12, 13... The 5<sup>th</sup> is 10.

**Constraints:**

*   <code>1 <= n, a, b, c <= 10<sup>9</sup></code>
*   <code>1 <= a * b * c <= 10<sup>18</sup></code>
*   It is guaranteed that the result will be in range <code>[1, 2 * 10<sup>9</sup>]</code>.

## Solution

```kotlin
class Solution {
    private fun getLcm(a: Long, b: Long): Long {
        var mx = a
        var mn = b
        if (a < b) {
            mx = b
            mn = a
        }
        while (mn != 0L) {
            val tmp = mn
            mn = mx % mn
            mx = tmp
        }
        return a * b / mx
    }

    fun nthUglyNumber(n: Int, a: Int, b: Int, c: Int): Int {
        val ab = getLcm(a.toLong(), b.toLong())
        val ac = getLcm(a.toLong(), c.toLong())
        val bc = getLcm(b.toLong(), c.toLong())
        val abc = getLcm(a.toLong(), bc)
        var left: Long = 1
        var right: Long = 2000000001
        if (a != 0 && b != 0 && c != 0 && bc != 0L) {
            while (left < right) {
                val mid = left + (right - left) / 2
                if (mid / a + mid / b + mid / c - mid / ab - mid / ac - mid / bc + mid / abc >= n) {
                    right = mid
                } else {
                    left = mid + 1
                }
            }
        }
        return left.toInt()
    }
}
```