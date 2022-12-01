[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 392\. Is Subsequence

Easy

Given two strings `s` and `t`, return `true` _if_ `s` _is a **subsequence** of_ `t`_, or_ `false` _otherwise_.

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of <code>"<ins>a</ins>b<ins>c</ins>d<ins>e</ins>"</code> while `"aec"` is not).

**Example 1:**

**Input:** s = "abc", t = "ahbgdc"

**Output:** true

**Example 2:**

**Input:** s = "axc", t = "ahbgdc"

**Output:** false

**Constraints:**

*   `0 <= s.length <= 100`
*   <code>0 <= t.length <= 10<sup>4</sup></code>
*   `s` and `t` consist only of lowercase English letters.

**Follow up:** Suppose there are lots of incoming `s`, say <code>s<sub>1</sub>, s<sub>2</sub>, ..., s<sub>k</sub></code> where <code>k >= 10<sup>9</sup></code>, and you want to check one by one to see if `t` has its subsequence. In this scenario, how would you change your code?

## Solution

```kotlin
class Solution {
    fun isSubsequence(s: String, t: String): Boolean {
        var i = 0
        var j = 0
        while (i < s.length && j < t.length) {
            if (s[i] == t[j]) i++
            j++
        }
        return i == s.length
    }
}
```