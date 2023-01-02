[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 479\. Largest Palindrome Product

Hard

Given an integer n, return _the **largest palindromic integer** that can be represented as the product of two `n`\-digits integers_. Since the answer can be very large, return it **modulo** `1337`.

**Example 1:**

**Input:** n = 2

**Output:** 987 Explanation: 99 x 91 = 9009, 9009 % 1337 = 987

**Example 2:**

**Input:** n = 1

**Output:** 9

**Constraints:**

*   `1 <= n <= 8`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun largestPalindrome(n: Int): Int {
        val pow10 = Math.pow(10.0, n.toDouble()).toLong()
        val max = (pow10 - 1) * (pow10 - Math.sqrt(pow10.toDouble()).toLong() + 1)
        val left = max / pow10
        var t = pow10 / 11
        t -= t.inv() and 1L
        for (i in left downTo 1) {
            var j = t
            val num = gen(i)
            while (j >= i / 11) {
                if (num % j == 0L) {
                    return (num % 1337).toInt()
                }
                j -= 2
            }
        }
        return 9
    }

    private fun gen(x: Long): Long {
        var x = x
        var r = x
        while (x > 0) {
            r = r * 10 + x % 10
            x /= 10
        }
        return r
    }
}
```