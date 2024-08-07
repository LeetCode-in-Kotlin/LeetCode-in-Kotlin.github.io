[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3233\. Find the Count of Numbers Which Are Not Special

Medium

You are given 2 **positive** integers `l` and `r`. For any number `x`, all positive divisors of `x` _except_ `x` are called the **proper divisors** of `x`.

A number is called **special** if it has exactly 2 **proper divisors**. For example:

*   The number 4 is _special_ because it has proper divisors 1 and 2.
*   The number 6 is _not special_ because it has proper divisors 1, 2, and 3.

Return the count of numbers in the range `[l, r]` that are **not** **special**.

**Example 1:**

**Input:** l = 5, r = 7

**Output:** 3

**Explanation:**

There are no special numbers in the range `[5, 7]`.

**Example 2:**

**Input:** l = 4, r = 16

**Output:** 11

**Explanation:**

The special numbers in the range `[4, 16]` are 4 and 9.

**Constraints:**

*   <code>1 <= l <= r <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.sqrt

class Solution {
    fun nonSpecialCount(l: Int, r: Int): Int {
        val primes = sieveOfEratosthenes(sqrt(r.toDouble()).toInt())
        var specialCount = 0

        for (prime in primes) {
            val primeSquare = prime.toLong() * prime
            if (primeSquare in l..r) {
                specialCount++
            }
        }

        val totalNumbersInRange = r - l + 1
        return totalNumbersInRange - specialCount
    }

    private fun sieveOfEratosthenes(n: Int): List<Int> {
        val isPrime = BooleanArray(n + 1)
        for (i in 2..n) {
            isPrime[i] = true
        }

        var p = 2
        while (p * p <= n) {
            if (isPrime[p]) {
                var i = p * p
                while (i <= n) {
                    isPrime[i] = false
                    i += p
                }
            }
            p++
        }

        val primes: MutableList<Int> = ArrayList()
        for (i in 2..n) {
            if (isPrime[i]) {
                primes.add(i)
            }
        }
        return primes
    }
}
```