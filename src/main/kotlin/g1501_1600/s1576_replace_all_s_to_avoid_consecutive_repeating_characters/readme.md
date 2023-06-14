[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1576\. Replace All ?'s to Avoid Consecutive Repeating Characters

Easy

Given a string `s` containing only lowercase English letters and the `'?'` character, convert **all** the `'?'` characters into lowercase letters such that the final string does not contain any **consecutive repeating** characters. You **cannot** modify the non `'?'` characters.

It is **guaranteed** that there are no consecutive repeating characters in the given string **except** for `'?'`.

Return _the final string after all the conversions (possibly zero) have been made_. If there is more than one solution, return **any of them**. It can be shown that an answer is always possible with the given constraints.

**Example 1:**

**Input:** s = "?zs"

**Output:** "azs"

**Explanation:** There are 25 solutions for this problem. From "azs" to "yzs", all are valid. Only "z" is an invalid modification as the string will consist of consecutive repeating characters in "zzs".

**Example 2:**

**Input:** s = "ubv?w"

**Output:** "ubvaw"

**Explanation:** There are 24 solutions for this problem. Only "v" and "w" are invalid modifications as the strings will consist of consecutive repeating characters in "ubvvw" and "ubvww".

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consist of lowercase English letters and `'?'`.

## Solution

```kotlin
class Solution {
    fun modifyString(s: String): String {
        val sb = StringBuilder()
        val len = s.length
        for (i in 0 until len) {
            val c = s[i]
            if (c == '?') {
                var replaceChar = 'a'
                val leftChar = if (i == 0) s[i] else sb[i - 1]
                val rightChar = s[Math.min(i + 1, len - 1)]
                while (replaceChar == leftChar || replaceChar == rightChar) {
                    replaceChar += 1.toChar().code
                }
                sb.append(replaceChar)
            } else {
                sb.append(c)
            }
        }
        return sb.toString()
    }
}
```