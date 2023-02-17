[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 664\. Strange Printer

Hard

There is a strange printer with the following two special properties:

*   The printer can only print a sequence of **the same character** each time.
*   At each turn, the printer can print new characters starting from and ending at any place and will cover the original existing characters.

Given a string `s`, return _the minimum number of turns the printer needed to print it_.

**Example 1:**

**Input:** s = "aaabbb"

**Output:** 2

**Explanation:** Print "aaa" first and then print "bbb".

**Example 2:**

**Input:** s = "aba"

**Output:** 2

**Explanation:** Print "aaa" first and then print "b" from the second place of the string, which will cover the existing character 'a'.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun strangePrinter(s: String): Int {
        if (s.isEmpty()) {
            return 0
        }
        val dp = Array(s.length) { IntArray(s.length) }
        for (i in s.length - 1 downTo 0) {
            for (j in i until s.length) {
                if (i == j) {
                    dp[i][j] = 1
                } else if (s[i] == s[i + 1]) {
                    dp[i][j] = dp[i + 1][j]
                } else {
                    dp[i][j] = dp[i + 1][j] + 1
                    for (k in i + 1..j) {
                        if (s[k] == s[i]) {
                            dp[i][j] = dp[i][j].coerceAtMost(dp[i + 1][k - 1] + dp[k][j])
                        }
                    }
                }
            }
        }
        return dp[0][s.length - 1]
    }
}
```