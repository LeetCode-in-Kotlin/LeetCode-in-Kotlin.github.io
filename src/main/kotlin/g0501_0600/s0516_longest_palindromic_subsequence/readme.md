[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 516\. Longest Palindromic Subsequence

Medium

Given a string `s`, find _the longest palindromic **subsequence**'s length in_ `s`.

A **subsequence** is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** s = "bbbab"

**Output:** 4

**Explanation:** One possible longest palindromic subsequence is "bbbb".

**Example 2:**

**Input:** s = "cbbd"

**Output:** 2

**Explanation:** One possible longest palindromic subsequence is "bb".

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun longestPalindromeSubseq(s: String): Int {
        if (s.isEmpty()) {
            return 0
        }
        val dp = Array(s.length) { IntArray(s.length) }
        for (jidiff in 0 until s.length) {
            for (i in 0 until s.length) {
                val j = i + jidiff
                if (j >= s.length) {
                    continue
                }
                dp[i][j] = when (j) {
                    i -> 1
                    i + 1 -> if (s[i] == s[j]) 2 else 1
                    else -> if (s[i] == s[j]) 2 + dp[i + 1][j - 1] else Math.max(dp[i + 1][j], dp[i][j - 1])
                }
            }
        }
        return dp[0][s.length - 1]
    }
}
```