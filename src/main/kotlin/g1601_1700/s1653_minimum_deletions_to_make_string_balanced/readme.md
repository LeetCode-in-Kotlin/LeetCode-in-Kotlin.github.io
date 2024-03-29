[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1653\. Minimum Deletions to Make String Balanced

Medium

You are given a string `s` consisting only of characters `'a'` and `'b'`.

You can delete any number of characters in `s` to make `s` **balanced**. `s` is **balanced** if there is no pair of indices `(i,j)` such that `i < j` and `s[i] = 'b'` and `s[j]= 'a'`.

Return _the **minimum** number of deletions needed to make_ `s` _**balanced**_.

**Example 1:**

**Input:** s = "aababbab"

**Output:** 2

**Explanation:** You can either:

Delete the characters at 0-indexed positions 2 and 6 ("aababbab" -> "aaabbb"), or 

Delete the characters at 0-indexed positions 3 and 6 ("aababbab" -> "aabbbb").

**Example 2:**

**Input:** s = "bbaaaaabb"

**Output:** 2

**Explanation:** The only solution is to delete the first two characters.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is `'a'` or `'b'`.

## Solution

```kotlin
class Solution {
    fun minimumDeletions(s: String): Int {
        var a = 0
        var b = 0
        for (ch in s.toCharArray()) {
            if (ch == 'a') {
                a++
            } else {
                b = Math.max(a, b) + 1
            }
        }
        return s.length - Math.max(a, b)
    }
}
```