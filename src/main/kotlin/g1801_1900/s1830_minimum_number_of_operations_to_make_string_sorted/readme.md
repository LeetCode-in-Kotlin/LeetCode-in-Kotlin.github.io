[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1830\. Minimum Number of Operations to Make String Sorted

Hard

You are given a string `s` (**0-indexed**). You are asked to perform the following operation on `s` until you get a sorted string:

1.  Find **the largest index** `i` such that `1 <= i < s.length` and `s[i] < s[i - 1]`.
2.  Find **the largest index** `j` such that `i <= j < s.length` and `s[k] < s[i - 1]` for all the possible values of `k` in the range `[i, j]` inclusive.
3.  Swap the two characters at indices `i - 1` and `j`.
4.  Reverse the suffix starting at index `i`.

Return _the number of operations needed to make the string sorted._ Since the answer can be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "cba"

**Output:** 5

**Explanation:** The simulation goes as follows: Operation 1: i=2, j=2. Swap s[1] and s[2] to get s="cab", then reverse the suffix starting at 2. Now, s="cab".

Operation 2: i=1, j=2. Swap s[0] and s[2] to get s="bac", then reverse the suffix starting at 1. Now, s="bca". 

Operation 3: i=2, j=2. Swap s[1] and s[2] to get s="bac", then reverse the suffix starting at 2. Now, s="bac".

Operation 4: i=1, j=1. Swap s[0] and s[1] to get s="abc", then reverse the suffix starting at 1. Now, s="acb". 

Operation 5: i=2, j=2. Swap s[1] and s[2] to get s="abc", then reverse the suffix starting at 2. Now, s="abc".

**Example 2:**

**Input:** s = "aabaa"

**Output:** 2

**Explanation:** The simulation goes as follows: 

Operation 1: i=3, j=4. Swap s[2] and s[4] to get s="aaaab", then reverse the substring starting at 3. Now, s="aaaba". 

Operation 2: i=4, j=4. Swap s[3] and s[4] to get s="aaaab", then reverse the substring starting at 4. Now, s="aaaab".

**Constraints:**

*   `1 <= s.length <= 3000`
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun makeStringSorted(s: String): Int {
        val n = s.length
        val count = IntArray(26)
        for (i in 0 until n) {
            count[s[i].code - 'a'.code]++
        }
        val fact = LongArray(n + 1)
        fact[0] = 1
        val mod = 1000000007
        for (i in 1..n) {
            fact[i] = fact[i - 1] * i % mod
        }
        var len = n
        var ans: Long = 0
        for (i in 0 until n) {
            len--
            val bound = s[i].code - 'a'.code
            var first = 0
            var rev: Long = 1
            for (k in 0..25) {
                if (k < bound) {
                    first += count[k]
                }
                rev = rev * fact[count[k]] % mod
            }
            ans = (
                ans % mod +
                    (
                        first * fact[len] % mod
                            * modPow(rev, mod.toLong() - 2, mod) %
                            mod
                        ) %
                    mod
                )
            ans %= mod
            count[bound]--
        }
        return ans.toInt()
    }

    private fun modPow(x: Long, n: Long, m: Int): Long {
        var x = x
        var n = n
        var result: Long = 1
        while (n > 0) {
            if (n and 1L != 0L) {
                result = result * x % m
            }
            x = x * x % m
            n = n shr 1
        }
        return result
    }
}
```