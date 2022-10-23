[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 204\. Count Primes

Medium

Given an integer `n`, return _the number of prime numbers that are strictly less than_ `n`.

**Example 1:**

**Input:** n = 10

**Output:** 4

**Explanation:** There are 4 prime numbers less than 10, they are 2, 3, 5, 7.

**Example 2:**

**Input:** n = 0

**Output:** 0

**Example 3:**

**Input:** n = 1

**Output:** 0

**Constraints:**

*   <code>0 <= n <= 5 * 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun countPrimes(n: Int): Int {
        val isprime = BooleanArray(n)
        var count = 0
        run {
            var i = 2
            while (i * i <= n) {
                if (!isprime[i]) {
                    var j = i * 2
                    while (j < n) {
                        isprime[j] = true
                        j += i
                    }
                }
                i++
            }
        }
        for (i in 2 until isprime.size) {
            if (!isprime[i]) {
                count++
            }
        }
        return count
    }
}
```