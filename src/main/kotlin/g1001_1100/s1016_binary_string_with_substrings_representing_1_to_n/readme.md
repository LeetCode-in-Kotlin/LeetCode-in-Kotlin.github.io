[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1016\. Binary String With Substrings Representing 1 To N

Medium

Given a binary string `s` and a positive integer `n`, return `true` _if the binary representation of all the integers in the range_ `[1, n]` _are **substrings** of_ `s`_, or_ `false` _otherwise_.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "0110", n = 3

**Output:** true

**Example 2:**

**Input:** s = "0110", n = 4

**Output:** false

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s[i]` is either `'0'` or `'1'`.
*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun queryString(s: String, n: Int): Boolean {
        for (i in 1..n) {
            val str = Integer.toBinaryString(i)
            if (!s.contains(str)) {
                return false
            }
        }
        return true
    }
}
```