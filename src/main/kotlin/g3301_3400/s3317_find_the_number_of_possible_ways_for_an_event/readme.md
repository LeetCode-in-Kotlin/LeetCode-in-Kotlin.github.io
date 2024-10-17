[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3317\. Find the Number of Possible Ways for an Event

Hard

You are given three integers `n`, `x`, and `y`.

An event is being held for `n` performers. When a performer arrives, they are **assigned** to one of the `x` stages. All performers assigned to the **same** stage will perform together as a band, though some stages _might_ remain **empty**.

After all performances are completed, the jury will **award** each band a score in the range `[1, y]`.

Return the **total** number of possible ways the event can take place.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Note** that two events are considered to have been held **differently** if **either** of the following conditions is satisfied:

*   **Any** performer is _assigned_ a different stage.
*   **Any** band is _awarded_ a different score.

**Example 1:**

**Input:** n = 1, x = 2, y = 3

**Output:** 6

**Explanation:**

*   There are 2 ways to assign a stage to the performer.
*   The jury can award a score of either 1, 2, or 3 to the only band.

**Example 2:**

**Input:** n = 5, x = 2, y = 1

**Output:** 32

**Explanation:**

*   Each performer will be assigned either stage 1 or stage 2.
*   All bands will be awarded a score of 1.

**Example 3:**

**Input:** n = 3, x = 3, y = 4

**Output:** 684

**Constraints:**

*   `1 <= n, x, y <= 1000`

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun numberOfWays(n: Int, x: Int, y: Int): Int {
        val fact = LongArray(x + 1)
        fact[0] = 1
        for (i in 1..x) {
            fact[i] = fact[i - 1] * i % MOD
        }
        val invFact = LongArray(x + 1)
        invFact[x] = powMod(fact[x], MOD - 2L)
        for (i in x - 1 downTo 0) {
            invFact[i] = invFact[i + 1] * (i + 1) % MOD
        }
        val powY = LongArray(x + 1)
        powY[0] = 1
        for (k in 1..x) {
            powY[k] = powY[k - 1] * y % MOD
        }
        val localArray = LongArray(x + 1)
        localArray[0] = 1
        for (i in 1..n) {
            val kMax: Int = min(i, x)
            for (k in kMax downTo 1) {
                localArray[k] = (k * localArray[k] + localArray[k - 1]) % MOD
            }
            localArray[0] = 0
        }
        var sum: Long = 0
        val kLimit: Int = min(n, x)
        for (k in 1..kLimit) {
            val localValue: Long = fact[x] * invFact[x - k] % MOD
            var term: Long = localValue * localArray[k] % MOD
            term = term * powY[k] % MOD
            sum = (sum + term) % MOD
        }
        return sum.toInt()
    }

    private fun powMod(a: Long, b: Long): Long {
        var a = a
        var b = b
        var res: Long = 1
        a = a % MOD
        while (b > 0) {
            if ((b and 1L) == 1L) {
                res = res * a % MOD
            }
            a = a * a % MOD
            b = b shr 1
        }
        return res
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```