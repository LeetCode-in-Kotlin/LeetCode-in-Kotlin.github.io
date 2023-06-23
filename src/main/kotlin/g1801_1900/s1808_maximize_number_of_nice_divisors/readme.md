[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1808\. Maximize Number of Nice Divisors

Hard

You are given a positive integer `primeFactors`. You are asked to construct a positive integer `n` that satisfies the following conditions:

*   The number of prime factors of `n` (not necessarily distinct) is **at most** `primeFactors`.
*   The number of nice divisors of `n` is maximized. Note that a divisor of `n` is **nice** if it is divisible by every prime factor of `n`. For example, if `n = 12`, then its prime factors are `[2,2,3]`, then `6` and `12` are nice divisors, while `3` and `4` are not.

Return _the number of nice divisors of_ `n`. Since that number can be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

Note that a prime number is a natural number greater than `1` that is not a product of two smaller natural numbers. The prime factors of a number `n` is a list of prime numbers such that their product equals `n`.

**Example 1:**

**Input:** primeFactors = 5

**Output:** 6

**Explanation:** 200 is a valid value of n. It has 5 prime factors: [2,2,2,5,5], and it has 6 nice divisors: [10,20,40,50,100,200]. There is not other value of n that has at most 5 prime factors and more nice divisors.

**Example 2:**

**Input:** primeFactors = 8

**Output:** 18

**Constraints:**

*   <code>1 <= primeFactors <= 10<sup>9</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private fun modPow(b: Long, e: Int, m: Int): Long {
        var b = b
        var e = e
        if (m == 1) {
            return 0
        }
        if (e == 0 || b == 1L) {
            return 1
        }
        b %= m.toLong()
        var r: Long = 1
        while (e > 0) {
            if (e and 1 == 1) {
                r = r * b % m
            }
            e = e shr 1
            b = b * b % m
        }
        return r
    }

    fun maxNiceDivisors(pf: Int): Int {
        val mod = 1000000007
        val st = intArrayOf(0, 1, 2, 3, 4, 6)
        return if (pf < 5) pf else (modPow(3, pf / 3 - 1, mod) * st[3 + pf % 3] % mod).toInt()
    }
}
```