[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 521\. Longest Uncommon Subsequence I

Easy

Given two strings `a` and `b`, return _the length of the **longest uncommon subsequence** between_ `a` _and_ `b`. If the longest uncommon subsequence does not exist, return `-1`.

An **uncommon subsequence** between two strings is a string that is a **subsequence of one but not the other**.

A **subsequence** of a string `s` is a string that can be obtained after deleting any number of characters from `s`.

*   For example, `"abc"` is a subsequence of `"aebdc"` because you can delete the underlined characters in <code>"a<ins>e</ins>b<ins>d</ins>c"</code> to get `"abc"`. Other subsequences of `"aebdc"` include `"aebdc"`, `"aeb"`, and `""` (empty string).

**Example 1:**

**Input:** a = "aba", b = "cdc"

**Output:** 3

**Explanation:** One longest uncommon subsequence is "aba" because "aba" is a subsequence of "aba" but not "cdc". Note that "cdc" is also a longest uncommon subsequence.

**Example 2:**

**Input:** a = "aaa", b = "bbb"

**Output:** 3

**Explanation:** The longest uncommon subsequences are "aaa" and "bbb".

**Example 3:**

**Input:** a = "aaa", b = "aaa"

**Output:** -1

**Explanation:** Every subsequence of string a is also a subsequence of string b. Similarly, every subsequence of string b is also a subsequence of string a.

**Constraints:**

*   `1 <= a.length, b.length <= 100`
*   `a` and `b` consist of lower-case English letters.

## Solution

```kotlin
class Solution {
    fun findLUSlength(a: String, b: String): Int {
        return if (a == b) {
            -1
        } else {
            a.length.coerceAtLeast(b.length)
        }
    }
}
```