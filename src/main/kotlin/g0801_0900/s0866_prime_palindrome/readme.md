[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 866\. Prime Palindrome

Medium

Given an integer n, return _the smallest **prime palindrome** greater than or equal to_ `n`.

An integer is **prime** if it has exactly two divisors: `1` and itself. Note that `1` is not a prime number.

*   For example, `2`, `3`, `5`, `7`, `11`, and `13` are all primes.

An integer is a **palindrome** if it reads the same from left to right as it does from right to left.

*   For example, `101` and `12321` are palindromes.

The test cases are generated so that the answer always exists and is in the range <code>[2, 2 * 10<sup>8</sup>]</code>.

**Example 1:**

**Input:** n = 6

**Output:** 7

**Example 2:**

**Input:** n = 8

**Output:** 11

**Example 3:**

**Input:** n = 13

**Output:** 101

**Constraints:**

*   <code>1 <= n <= 10<sup>8</sup></code>

## Solution

```kotlin
import kotlin.math.sqrt

@Suppress("NAME_SHADOWING")
class Solution {
    private fun isPrime(n: Int): Boolean {
        if (n % 2 == 0) {
            return n == 2
        }
        var i = 3
        val s = sqrt(n.toDouble()).toInt()
        while (i <= s) {
            if (n % i == 0) {
                return false
            }
            i += 2
        }
        return true
    }

    private fun next(num: Int): Int {
        val s = (num + 1).toString().toCharArray()
        var i = 0
        val n = s.size
        while (i < n shr 1) {
            while (s[i] != s[n - 1 - i]) {
                increment(s, n - 1 - i)
            }
            i++
        }
        return String(s).toInt()
    }

    private fun increment(s: CharArray, i: Int) {
        var i = i
        while (s[i] == '9') {
            s[i--] = '0'
        }
        s[i]++
    }

    fun primePalindrome(n: Int): Int {
        if (n <= 2) {
            return 2
        }
        var p = next(n - 1)
        while (!isPrime(p)) {
            if (p in 10000000..99999999) {
                p = 100000000
            }
            p = next(p)
        }
        return p
    }
}
```