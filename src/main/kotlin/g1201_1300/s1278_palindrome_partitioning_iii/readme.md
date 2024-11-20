[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1278\. Palindrome Partitioning III

Hard

You are given a string `s` containing lowercase letters and an integer `k`. You need to :

*   First, change some characters of `s` to other lowercase English letters.
*   Then divide `s` into `k` non-empty disjoint substrings such that each substring is a palindrome.

Return _the minimal number of characters that you need to change to divide the string_.

**Example 1:**

**Input:** s = "abc", k = 2

**Output:** 1

**Explanation:** You can split the string into "ab" and "c", and change 1 character in "ab" to make it palindrome.

**Example 2:**

**Input:** s = "aabbc", k = 3

**Output:** 0

**Explanation:** You can split the string into "aa", "bb" and "c", all of them are palindrome.

**Example 3:**

**Input:** s = "leetcode", k = 8

**Output:** 0

**Constraints:**

*   `1 <= k <= s.length <= 100`.
*   `s` only contains lowercase English letters.

## Solution

```kotlin
class Solution {
    fun palindromePartition(s: String, k: Int): Int {
        val n = s.length
        val pal = Array(n + 1) { IntArray(n + 1) }
        fillPal(s, n, pal)
        val dp = Array(n + 1) { IntArray(k + 1) }
        for (row in dp) {
            row.fill(-1)
        }
        return calculateMinCost(s, 0, n, k, pal, dp)
    }

    private fun calculateMinCost(
        s: String,
        index: Int,
        n: Int,
        k: Int,
        pal: Array<IntArray>,
        dp: Array<IntArray>,
    ): Int {
        if (index == n) {
            return n
        }
        if (k == 1) {
            return pal[index][n - 1]
        }
        if (dp[index][k] != -1) {
            return dp[index][k]
        }
        var ans = Int.MAX_VALUE
        for (i in index until n) {
            ans = Math.min(ans, pal[index][i] + calculateMinCost(s, i + 1, n, k - 1, pal, dp))
        }
        dp[index][k] = ans
        return ans
    }

    private fun fillPal(s: String, n: Int, pal: Array<IntArray>) {
        for (gap in 0 until n) {
            var i = 0
            var j = gap
            while (j < n) {
                if (gap == 0) {
                    pal[i][i] = 0
                } else if (gap == 1) {
                    if (s[i] == s[i + 1]) {
                        pal[i][i + 1] = 0
                    } else {
                        pal[i][i + 1] = 1
                    }
                } else {
                    if (s[i] == s[j]) {
                        pal[i][j] = pal[i + 1][j - 1]
                    } else {
                        pal[i][j] = pal[i + 1][j - 1] + 1
                    }
                }
                i++
                j++
            }
        }
    }
}
```