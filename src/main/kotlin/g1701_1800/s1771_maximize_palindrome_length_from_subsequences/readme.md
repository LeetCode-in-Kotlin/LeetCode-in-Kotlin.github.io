[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1771\. Maximize Palindrome Length From Subsequences

Hard

You are given two strings, `word1` and `word2`. You want to construct a string in the following manner:

*   Choose some **non-empty** subsequence `subsequence1` from `word1`.
*   Choose some **non-empty** subsequence `subsequence2` from `word2`.
*   Concatenate the subsequences: `subsequence1 + subsequence2`, to make the string.

Return _the **length** of the longest **palindrome** that can be constructed in the described manner._ If no palindromes can be constructed, return `0`.

A **subsequence** of a string `s` is a string that can be made by deleting some (possibly none) characters from `s` without changing the order of the remaining characters.

A **palindrome** is a string that reads the same forward as well as backward.

**Example 1:**

**Input:** word1 = "cacb", word2 = "cbba"

**Output:** 5

**Explanation:** Choose "ab" from word1 and "cba" from word2 to make "abcba", which is a palindrome.

**Example 2:**

**Input:** word1 = "ab", word2 = "ab"

**Output:** 3

**Explanation:** Choose "ab" from word1 and "a" from word2 to make "aba", which is a palindrome.

**Example 3:**

**Input:** word1 = "aa", word2 = "bb"

**Output:** 0

**Explanation:** You cannot construct a palindrome from the described method, so return 0.

**Constraints:**

*   `1 <= word1.length, word2.length <= 1000`
*   `word1` and `word2` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun longestPalindrome(word1: String, word2: String): Int {
        val len1 = word1.length
        val len2 = word2.length
        val len = len1 + len2
        val word = word1 + word2
        val dp = Array(len) { IntArray(len) }
        var max = 0
        val arr = word.toCharArray()
        for (d in 1..len) {
            var i = 0
            while (i + d - 1 < len) {
                if (arr[i] == arr[i + d - 1]) {
                    dp[i][i + d - 1] = if (d == 1) 1 else Math.max(dp[i + 1][i + d - 2] + 2, dp[i][i + d - 1])
                    if (i < len1 && i + d - 1 >= len1) {
                        max = Math.max(max, dp[i][i + d - 1])
                    }
                } else {
                    dp[i][i + d - 1] = Math.max(dp[i + 1][i + d - 1], dp[i][i + d - 2])
                }
                i++
            }
        }
        return max
    }
}
```