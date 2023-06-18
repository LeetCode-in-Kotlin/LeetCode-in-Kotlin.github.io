[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1638\. Count Substrings That Differ by One Character

Medium

Given two strings `s` and `t`, find the number of ways you can choose a non-empty substring of `s` and replace a **single character** by a different character such that the resulting substring is a substring of `t`. In other words, find the number of substrings in `s` that differ from some substring in `t` by **exactly** one character.

For example, the underlined substrings in `"computer"` and `"computation"` only differ by the `'e'`/`'a'`, so this is a valid way.

Return _the number of substrings that satisfy the condition above._

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "aba", t = "baba"

**Output:** 6

**Explanation:** The following are the pairs of substrings from s and t that differ by exactly 1 character: 

("aba", "baba") 

("aba", "baba")

("aba", "baba") 

("aba", "baba")

("aba", "baba") 

("aba", "baba") 

The underlined portions are the substrings that are chosen from s and t.

**Example 2:**

**Input:** s = "ab", t = "bb"

**Output:** 3

**Explanation:** The following are the pairs of substrings from s and t that differ by 1 character: 

("ab", "bb") 

("ab", "bb")

("ab", "bb") 

The underlined portions are the substrings that are chosen from s and t.

**Constraints:**

*   `1 <= s.length, t.length <= 100`
*   `s` and `t` consist of lowercase English letters only.

## Solution

```kotlin
class Solution {
    fun countSubstrings(s: String, t: String): Int {
        var ans = 0
        val n = s.length
        val m = t.length
        val dp = Array(n + 1) { IntArray(m + 1) }
        val tp = Array(n + 1) { IntArray(m + 1) }
        for (i in n - 1 downTo 0) {
            for (j in m - 1 downTo 0) {
                if (s[i] == t[j]) {
                    dp[i][j] = dp[i + 1][j + 1] + 1
                    tp[i][j] = tp[i + 1][j + 1]
                } else {
                    tp[i][j] = dp[i + 1][j + 1] + 1
                }
                ans += tp[i][j]
            }
        }
        return ans
    }
}
```