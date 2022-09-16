[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1143\. Longest Common Subsequence

Medium

Given two strings `text1` and `text2`, return _the length of their longest **common subsequence**._ If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

*   For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1:**

**Input:** text1 = "abcde", text2 = "ace"

**Output:** 3

**Explanation:** The longest common subsequence is "ace" and its length is 3.

**Example 2:**

**Input:** text1 = "abc", text2 = "abc"

**Output:** 3

**Explanation:** The longest common subsequence is "abc" and its length is 3.

**Example 3:**

**Input:** text1 = "abc", text2 = "def"

**Output:** 0

**Explanation:** There is no such common subsequence, so the result is 0.

**Constraints:**

*   `1 <= text1.length, text2.length <= 1000`
*   `text1` and `text2` consist of only lowercase English characters.

## Solution

```kotlin
class Solution {
    fun longestCommonSubsequence(text1: String, text2: String): Int {
        val n = text1.length
        val m = text2.length
        val dp = Array(n + 1) { IntArray(m + 1) }
        for (i in 1..n) {
            for (j in 1..m) {
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1])
                }
            }
        }
        return dp[n][m]
    }
}
```