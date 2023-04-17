[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 903\. Valid Permutations for DI Sequence

Hard

You are given a string `s` of length `n` where `s[i]` is either:

*   `'D'` means decreasing, or
*   `'I'` means increasing.

A permutation `perm` of `n + 1` integers of all the integers in the range `[0, n]` is called a **valid permutation** if for all valid `i`:

*   If `s[i] == 'D'`, then `perm[i] > perm[i + 1]`, and
*   If `s[i] == 'I'`, then `perm[i] < perm[i + 1]`.

Return _the number of **valid permutations**_ `perm`. Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "DID"

**Output:** 5

**Explanation:** The 5 valid permutations of (0, 1, 2, 3) are: 

    (1, 0, 3, 2) 
    (2, 0, 3, 1) 
    (2, 1, 3, 0) 
    (3, 0, 2, 1) 
    (3, 1, 2, 0)

**Example 2:**

**Input:** s = "D"

**Output:** 1

**Constraints:**

*   `n == s.length`
*   `1 <= n <= 200`
*   `s[i]` is either `'I'` or `'D'`.

## Solution

```kotlin
class Solution {
    fun numPermsDISequence(s: String): Int {
        val n = s.length
        val mod = 1e9.toInt() + 7
        val dp = Array(n + 1) { IntArray(n + 1) }
        for (j in 0..n) {
            dp[0][j] = 1
        }
        for (i in 0 until n) {
            var cur = 0
            if (s[i] == 'I') {
                for (j in 0 until n - i) {
                    cur = (cur + dp[i][j]) % mod
                    dp[i + 1][j] = cur
                }
            } else {
                for (j in n - i - 1 downTo 0) {
                    cur = (cur + dp[i][j + 1]) % mod
                    dp[i + 1][j] = cur
                }
            }
        }
        return dp[n][0]
    }
}
```