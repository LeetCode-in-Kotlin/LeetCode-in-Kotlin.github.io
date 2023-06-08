[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1312\. Minimum Insertion Steps to Make a String Palindrome

Hard

Given a string `s`. In one step you can insert any character at any index of the string.

Return _the minimum number of steps_ to make `s` palindrome.

A **Palindrome String** is one that reads the same backward as well as forward.

**Example 1:**

**Input:** s = "zzazz"

**Output:** 0

**Explanation:** The string "zzazz" is already palindrome we don't need any insertions.

**Example 2:**

**Input:** s = "mbadm"

**Output:** 2

**Explanation:** String can be "mbdadbm" or "mdbabdm".

**Example 3:**

**Input:** s = "leetcode"

**Output:** 5

**Explanation:** Inserting 5 characters the string becomes "leetcodocteel".

**Constraints:**

*   `1 <= s.length <= 500`
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    private fun longestPalindrome(a: String, b: String, n: Int): Int {
        val dp = Array(n + 1) { IntArray(n + 1) }
        for (i in 0 until n + 1) {
            for (j in 0 until n + 1) {
                if (i == 0 || j == 0) {
                    dp[i][j] = 0
                } else if (a[i - 1] == b[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1]
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1])
                }
            }
        }
        return dp[n][n]
    }

    fun minInsertions(s: String): Int {
        val n = s.length
        if (n < 2) {
            return 0
        }
        val rs = StringBuilder(s).reverse().toString()
        val l = longestPalindrome(s, rs, n)
        return n - l
    }
}
```