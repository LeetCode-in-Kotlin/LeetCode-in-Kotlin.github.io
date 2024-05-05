[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3130\. Find All Possible Stable Binary Arrays II

Hard

You are given 3 positive integers `zero`, `one`, and `limit`.

A binary array `arr` is called **stable** if:

*   The number of occurrences of 0 in `arr` is **exactly** `zero`.
*   The number of occurrences of 1 in `arr` is **exactly** `one`.
*   Each subarray of `arr` with a size greater than `limit` must contain **both** 0 and 1.

Return the _total_ number of **stable** binary arrays.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** zero = 1, one = 1, limit = 2

**Output:** 2

**Explanation:**

The two possible stable binary arrays are `[1,0]` and `[0,1]`.

**Example 2:**

**Input:** zero = 1, one = 2, limit = 1

**Output:** 1

**Explanation:**

The only possible stable binary array is `[1,0,1]`.

**Example 3:**

**Input:** zero = 3, one = 3, limit = 2

**Output:** 14

**Explanation:**

All the possible stable binary arrays are `[0,0,1,0,1,1]`, `[0,0,1,1,0,1]`, `[0,1,0,0,1,1]`, `[0,1,0,1,0,1]`, `[0,1,0,1,1,0]`, `[0,1,1,0,0,1]`, `[0,1,1,0,1,0]`, `[1,0,0,1,0,1]`, `[1,0,0,1,1,0]`, `[1,0,1,0,0,1]`, `[1,0,1,0,1,0]`, `[1,0,1,1,0,0]`, `[1,1,0,0,1,0]`, and `[1,1,0,1,0,0]`.

**Constraints:**

*   `1 <= zero, one, limit <= 1000`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    private var factorial: LongArray? = null
    private lateinit var reverse: LongArray

    fun numberOfStableArrays(zero: Int, one: Int, limit: Int): Int {
        if (factorial == null) {
            factorial = LongArray(N + 1)
            reverse = LongArray(N + 1)
            factorial!![0] = 1
            reverse[0] = 1
            var x: Long = 1
            for (i in 1..N) {
                x = (x * i) % MOD
                factorial!![i] = x.toInt().toLong()
                reverse[i] = getInverse(x, MOD.toLong())
            }
        }
        var ans: Long = 0
        val s = LongArray(one + 1)
        val n = (min(zero, one) + 1).toInt()
        for (
            groups0 in (zero + limit - 1) / limit..min(zero, n)
                .toInt()
        ) {
            val s0 = calc(groups0, zero, limit)
            for (
                groups1 in max(
                    groups0 - 1,
                    (one + limit - 1) / limit
                )..min((groups0 + 1), one)
            ) {
                var s1: Long
                if (s[groups1] != 0L) {
                    s1 = s[groups1]
                } else {
                    s[groups1] = calc(groups1, one, limit)
                    s1 = s[groups1]
                }
                ans = (ans + s0 * s1 * (if (groups1 == groups0) 2 else 1)) % MOD
            }
        }
        return ((ans + MOD) % MOD).toInt()
    }

    fun calc(groups: Int, x: Int, limit: Int): Long {
        var s: Long = 0
        var sign = 1
        var k = 0
        while (k * limit <= x - groups && k <= groups) {
            s = (s + sign * comb(groups, k) * comb(x - k * limit - 1, groups - 1)) % MOD
            sign *= -1
            k++
        }
        return s
    }

    fun comb(n: Int, k: Int): Long {
        return (factorial!![n] * reverse[k] % MOD) * reverse[n - k] % MOD
    }

    fun getInverse(n: Long, mod: Long): Long {
        var n = n
        var p = mod
        var x: Long = 1
        var y: Long = 0
        while (p > 0) {
            val quotient = n / p
            val remainder = n % p
            val tempY = x - quotient * y
            x = y
            y = tempY
            n = p
            p = remainder
        }
        return ((x % mod) + mod) % mod
    }

    companion object {
        private const val MOD = 1e9.toInt() + 7
        private const val N = 1000
    }
}
```