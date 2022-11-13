[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 313\. Super Ugly Number

Medium

A **super ugly number** is a positive integer whose prime factors are in the array `primes`.

Given an integer `n` and an array of integers `primes`, return _the_ <code>n<sup>th</sup></code> _**super ugly number**_.

The <code>n<sup>th</sup></code> **super ugly number** is **guaranteed** to fit in a **32-bit** signed integer.

**Example 1:**

**Input:** n = 12, primes = [2,7,13,19]

**Output:** 32

**Explanation:** [1,2,4,7,8,13,14,16,19,26,28,32] is the sequence of the first 12 super ugly numbers given primes = [2,7,13,19].

**Example 2:**

**Input:** n = 1, primes = [2,3,5]

**Output:** 1

**Explanation:** 1 has no prime factors, therefore all of its prime factors are in the array primes = [2,3,5].

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `1 <= primes.length <= 100`
*   `2 <= primes[i] <= 1000`
*   `primes[i]` is **guaranteed** to be a prime number.
*   All the values of `primes` are **unique** and sorted in **ascending order**.

## Solution

```kotlin
class Solution {
    fun nthSuperUglyNumber(n: Int, primes: IntArray): Int {
        val primes1 = LongArray(primes.size)
        for (i in primes.indices) {
            primes1[i] = primes[i].toLong()
        }
        val index = IntArray(primes.size)
        val n1 = LongArray(n)
        n1[0] = 1L
        for (i in 1 until n) {
            var min = Long.MAX_VALUE
            for (l in primes1) {
                min = Math.min(min, l)
            }
            n1[i] = min
            for (j in primes1.indices) {
                if (min == primes1[j]) {
                    primes1[j] = primes[j] * n1[++index[j]]
                }
            }
        }
        return n1[n - 1].toInt()
    }
}
```