[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 680\. Valid Palindrome II

Easy

Given a string `s`, return `true` _if the_ `s` _can be palindrome after deleting **at most one** character from it_.

**Example 1:**

**Input:** s = "aba"

**Output:** true

**Example 2:**

**Input:** s = "abca"

**Output:** true

**Explanation:** You could delete the character 'c'.

**Example 3:**

**Input:** s = "abc"

**Output:** false

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun validPalindrome(s: String): Boolean {
        var l = 0
        var r = s.length - 1
        while (l < r) {
            if (s[l] != s[r]) {
                return isPalindrome(s.substring(l + 1, r + 1)) || isPalindrome(s.substring(l, r))
            }
            l++
            r--
        }
        return true
    }

    private fun isPalindrome(s: String): Boolean {
        var l = 0
        var r = s.length - 1
        while (l < r) {
            if (s[l] != s[r]) {
                return false
            }
            l++
            r--
        }
        return true
    }
}
```