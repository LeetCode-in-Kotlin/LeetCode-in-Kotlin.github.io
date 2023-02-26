[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 712\. Minimum ASCII Delete Sum for Two Strings

Medium

Given two strings `s1` and `s2`, return _the lowest **ASCII** sum of deleted characters to make two strings equal_.

**Example 1:**

**Input:** s1 = "sea", s2 = "eat"

**Output:** 231

**Explanation:** Deleting "s" from "sea" adds the ASCII value of "s" (115) to the sum. Deleting "t" from "eat" adds 116 to the sum. At the end, both strings are equal, and 115 + 116 = 231 is the minimum sum possible to achieve this.

**Example 2:**

**Input:** s1 = "delete", s2 = "leet"

**Output:** 403

**Explanation:** Deleting "dee" from "delete" to turn the string into "let", adds 100[d] + 101[e] + 101[e] to the sum. Deleting "e" from "leet" adds 101[e] to the sum. At the end, both strings are equal to "let", and the answer is 100+101+101+101 = 403. If instead we turned both strings into "lee" or "eet", we would get answers of 433 or 417, which are higher.

**Constraints:**

*   `1 <= s1.length, s2.length <= 1000`
*   `s1` and `s2` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun minimumDeleteSum(s1: String, s2: String): Int {
        val len1 = s1.length
        val len2 = s2.length
        val dp = Array(len1 + 1) { IntArray(len2 + 1) }
        var c1: Char
        var c2: Char
        for (i in 1 until len1 + 1) {
            dp[i][0] = dp[i - 1][0] + s1[i - 1].code
        }
        for (j in 1 until len2 + 1) {
            dp[0][j] = dp[0][j - 1] + s2[j - 1].code
        }
        for (i in 1 until len1 + 1) {
            c1 = s1[i - 1]
            for (j in 1 until len2 + 1) {
                c2 = s2[j - 1]
                if (c1 == c2) {
                    dp[i][j] = dp[i - 1][j - 1]
                } else {
                    dp[i][j] = (dp[i - 1][j] + c1.code).coerceAtMost(dp[i][j - 1] + c2.code)
                }
            }
        }
        return dp[len1][len2]
    }
}
```