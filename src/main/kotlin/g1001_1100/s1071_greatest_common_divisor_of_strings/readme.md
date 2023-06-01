[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1071\. Greatest Common Divisor of Strings

Easy

For two strings `s` and `t`, we say "`t` divides `s`" if and only if `s = t + ... + t` (i.e., `t` is concatenated with itself one or more times).

Given two strings `str1` and `str2`, return _the largest string_ `x` _such that_ `x` _divides both_ `str1` _and_ `str2`.

**Example 1:**

**Input:** str1 = "ABCABC", str2 = "ABC"

**Output:** "ABC"

**Example 2:**

**Input:** str1 = "ABABAB", str2 = "ABAB"

**Output:** "AB"

**Example 3:**

**Input:** str1 = "LEET", str2 = "CODE"

**Output:** ""

**Constraints:**

*   `1 <= str1.length, str2.length <= 1000`
*   `str1` and `str2` consist of English uppercase letters.

## Solution

```kotlin
class Solution {
    fun gcdOfStrings(str1: String?, str2: String?): String {
        if (str1 == null || str2 == null) {
            return ""
        }
        if (str1 == str2) {
            return str1
        }
        val m = str1.length
        val n = str2.length
        if (m > n && str1.substring(0, n) == str2) {
            return gcdOfStrings(str1.substring(n), str2)
        }
        return if (n > m && str2.substring(0, m) == str1) {
            gcdOfStrings(str2.substring(m), str1)
        } else ""
    }
}
```