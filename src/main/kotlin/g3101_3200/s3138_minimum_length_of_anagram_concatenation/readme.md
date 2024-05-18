[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3138\. Minimum Length of Anagram Concatenation

Medium

You are given a string `s`, which is known to be a concatenation of **anagrams** of some string `t`.

Return the **minimum** possible length of the string `t`.

An **anagram** is formed by rearranging the letters of a string. For example, "aab", "aba", and, "baa" are anagrams of "aab".

**Example 1:**

**Input:** s = "abba"

**Output:** 2

**Explanation:**

One possible string `t` could be `"ba"`.

**Example 2:**

**Input:** s = "cdef"

**Output:** 4

**Explanation:**

One possible string `t` could be `"cdef"`, notice that `t` can be equal to `s`.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consist only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.sqrt

class Solution {
    fun minAnagramLength(s: String): Int {
        val n = s.length
        val sq = IntArray(n)
        for (i in s.indices) {
            val ch = s[i].code
            if (i == 0) {
                sq[i] = ch * ch
            } else {
                sq[i] = sq[i - 1] + ch * ch
            }
        }
        val factors = getAllFactorsVer2(n)
        factors.sort()
        for (j in factors.indices) {
            val factor = factors[j]
            if (factor == 1) {
                if (sq[0] * n == sq[n - 1]) {
                    return 1
                }
            } else {
                val sum = sq[factor - 1]
                var start = 0
                var i = factor - 1
                while (i < n) {
                    if (start + sum != sq[i]) {
                        break
                    }
                    start += sum
                    if (i == n - 1) {
                        return factor
                    }
                    i += factor
                }
            }
        }
        return n - 1
    }

    private fun getAllFactorsVer2(n: Int): MutableList<Int> {
        val factors: MutableList<Int> = ArrayList()
        var i = 1
        while (i <= sqrt(n.toDouble())) {
            if (n % i == 0) {
                factors.add(i)
                factors.add(n / i)
            }
            i++
        }
        return factors
    }
}
```