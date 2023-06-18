[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1745\. Palindrome Partitioning IV

Hard

Given a string `s`, return `true` _if it is possible to split the string_ `s` _into three **non-empty** palindromic substrings. Otherwise, return_ `false`.

A string is said to be palindrome if it the same string when reversed.

**Example 1:**

**Input:** s = "abcbdd"

**Output:** true

**Explanation:** "abcbdd" = "a" + "bcb" + "dd", and all three substrings are palindromes.

**Example 2:**

**Input:** s = "bcbddxy"

**Output:** false

**Explanation:** s cannot be split into 3 palindromes.

**Constraints:**

*   `3 <= s.length <= 2000`
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun checkPartitioning(s: String): Boolean {
        val len: Int = s.length
        val ch: CharArray = s.toCharArray()
        val dp = IntArray(len + 1)
        dp[0] = 0x01
        for (i in 0 until len) {
            for (l: Int in intArrayOf(i - 1, i)) {
                var r: Int = i
                var localL = l
                while ((localL >= 0) && (r < len) && (ch[localL] == ch[r])) {
                    dp[r + 1] = dp[r + 1] or (dp[localL] shl 1)
                    localL--
                    r++
                }
            }
        }
        return (dp[len] and 0x08) == 0x08
    }
}
```