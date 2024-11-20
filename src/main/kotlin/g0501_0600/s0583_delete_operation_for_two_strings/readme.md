[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 583\. Delete Operation for Two Strings

Medium

Given two strings `word1` and `word2`, return _the minimum number of **steps** required to make_ `word1` _and_ `word2` _the same_.

In one **step**, you can delete exactly one character in either string.

**Example 1:**

**Input:** word1 = "sea", word2 = "eat"

**Output:** 2

**Explanation:** You need one step to make "sea" to "ea" and another step to make "eat" to "ea".

**Example 2:**

**Input:** word1 = "leetcode", word2 = "etco"

**Output:** 4

**Constraints:**

*   `1 <= word1.length, word2.length <= 500`
*   `word1` and `word2` consist of only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun minDistance(word1: String, word2: String): Int {
        val m = word1.length
        val n = word2.length
        val dp = Array(m + 1) { IntArray(n + 1) }
        for (i in 1..m) {
            for (j in 1..n) {
                dp[i][j] = if (word1[i - 1] == word2[j - 1]) {
                    dp[i - 1][j - 1] + 1
                } else {
                    Math.max(
                        dp[i - 1][j],
                        dp[i][j - 1],
                    )
                }
            }
        }
        return m + n - 2 * dp[m][n]
    }
}
```