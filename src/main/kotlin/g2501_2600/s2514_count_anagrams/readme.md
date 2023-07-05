[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2514\. Count Anagrams

Hard

You are given a string `s` containing one or more words. Every consecutive pair of words is separated by a single space `' '`.

A string `t` is an **anagram** of string `s` if the <code>i<sup>th</sup></code> word of `t` is a **permutation** of the <code>i<sup>th</sup></code> word of `s`.

*   For example, `"acb dfe"` is an anagram of `"abc def"`, but `"def cab"` and `"adc bef"` are not.

Return _the number of **distinct anagrams** of_ `s`. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "too hot"

**Output:** 18

**Explanation:** Some of the anagrams of the given string are "too hot", "oot hot", "oto toh", "too toh", and "too oht".

**Example 2:**

**Input:** s = "aa"

**Output:** 1

**Explanation:** There is only one anagram possible for the given string.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters and spaces `' '`.
*   There is single space between consecutive words.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun countAnagrams(s: String): Int {
        val charArray = s.toCharArray()
        var ans = 1L
        var mul = 1L
        val cnt = IntArray(26)
        var j = 0
        for (c in charArray) {
            if (c == ' ') {
                cnt.fill(0)
                j = 0
            } else {
                mul = mul * ++cnt[c.code - 'a'.code] % MOD
                ans = ans * ++j % MOD
            }
        }
        return (ans * pow(mul, MOD - 2) % MOD).toInt()
    }

    private fun pow(x: Long, n: Int): Long {
        var x = x
        var n = n
        var res = 1L
        while (n > 0) {
            if (n % 2 > 0) {
                res = res * x % MOD
            }
            x = x * x % MOD
            n /= 2
        }
        return res
    }

    companion object {
        private const val MOD = 1e9.toInt() + 7
    }
}
```