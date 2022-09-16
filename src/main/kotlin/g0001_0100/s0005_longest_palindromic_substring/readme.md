[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 5\. Longest Palindromic Substring

Medium

Given a string `s`, return _the longest palindromic substring_ in `s`.

**Example 1:**

**Input:** s = "babad"

**Output:** "bab" **Note:** "aba" is also a valid answer. 

**Example 2:**

**Input:** s = "cbbd"

**Output:** "bb" 

**Example 3:**

**Input:** s = "a"

**Output:** "a" 

**Example 4:**

**Input:** s = "ac"

**Output:** "a" 

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consist of only digits and English letters.

## Solution

```kotlin
class Solution {
    fun longestPalindrome(s: String): String {
        val newStr = CharArray(s.length * 2 + 1)
        newStr[0] = '#'
        for (i in s.indices) {
            newStr[2 * i + 1] = s[i]
            newStr[2 * i + 2] = '#'
        }
        val dp = IntArray(newStr.size)
        var friendCenter = 0
        var friendRadius = 0
        var lpsCenter = 0
        var lpsRadius = 0
        for (i in newStr.indices) {
            dp[i] = if (friendCenter + friendRadius > i) Math.min(
                dp[friendCenter * 2 - i],
                friendCenter + friendRadius - i
            ) else 1
            while (i + dp[i] < newStr.size && i - dp[i] >= 0 && newStr[i + dp[i]] == newStr[i - dp[i]]) {
                dp[i]++
            }
            if (friendCenter + friendRadius < i + dp[i]) {
                friendCenter = i
                friendRadius = dp[i]
            }
            if (lpsRadius < dp[i]) {
                lpsCenter = i
                lpsRadius = dp[i]
            }
        }
        return s.substring((lpsCenter - lpsRadius + 1) / 2, (lpsCenter + lpsRadius - 1) / 2)
    }
}
```