[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1092\. Shortest Common Supersequence

Hard

Given two strings `str1` and `str2`, return _the shortest string that has both_ `str1` _and_ `str2` _as **subsequences**_. If there are multiple valid strings, return **any** of them.

A string `s` is a **subsequence** of string `t` if deleting some number of characters from `t` (possibly `0`) results in the string `s`.

**Example 1:**

**Input:** str1 = "abac", str2 = "cab"

**Output:** "cabac"

**Explanation:** 

str1 = "abac" is a subsequence of "cabac" because we can delete the first "c". 

str2 = "cab" is a subsequence of "cabac" because we can delete the last "ac". 

The answer provided is the shortest such string that satisfies these properties.

**Example 2:**

**Input:** str1 = "aaaaaaaa", str2 = "aaaaaaaa"

**Output:** "aaaaaaaa"

**Constraints:**

*   `1 <= str1.length, str2.length <= 1000`
*   `str1` and `str2` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun shortestCommonSupersequence(str1: String, str2: String): String {
        val m = str1.length
        val n = str2.length
        val dp = Array(m + 1) { IntArray(n + 1) }
        for (i in 0..m) {
            for (j in 0..n) {
                if (i == 0) {
                    dp[i][j] = j
                } else if (j == 0) {
                    dp[i][j] = i
                } else if (str1[i - 1] == str2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1]
                } else {
                    dp[i][j] = 1 + Math.min(dp[i - 1][j], dp[i][j - 1])
                }
            }
        }
        // Length of the ShortestSuperSequence
        var l = dp[m][n]
        val arr = CharArray(l)
        var i = m
        var j = n
        while (i > 0 && j > 0) {
            /* If current character in str1 and str2 are same, then
            current character is part of shortest supersequence */
            if (str1[i - 1] == str2[j - 1]) {
                arr[--l] = str1[i - 1]
                i--
                j--
            } else if (dp[i - 1][j] < dp[i][j - 1]) {
                arr[--l] = str1[i - 1]
                i--
            } else {
                arr[--l] = str2[j - 1]
                j--
            }
        }
        while (i > 0) {
            arr[--l] = str1[i - 1]
            i--
        }
        while (j > 0) {
            arr[--l] = str2[j - 1]
            j--
        }
        return String(arr)
    }
}
```