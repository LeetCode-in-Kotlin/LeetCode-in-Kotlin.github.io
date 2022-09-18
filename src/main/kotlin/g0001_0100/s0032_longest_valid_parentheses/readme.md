[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 32\. Longest Valid Parentheses

Hard

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

**Example 1:**

**Input:** s = "(()"

**Output:** 2

**Explanation:** The longest valid parentheses substring is "()".

**Example 2:**

**Input:** s = ")()())"

**Output:** 4

**Explanation:** The longest valid parentheses substring is "()()".

**Example 3:**

**Input:** s = ""

**Output:** 0

**Constraints:**

*   <code>0 <= s.length <= 3 * 10<sup>4</sup></code>
*   `s[i]` is `'('`, or `')'`.

## Solution

```kotlin
class Solution {
    fun longestValidParentheses(s: String): Int {
        var max = 0
        var left = 0
        var right = 0
        val n = s.length
        var ch: Char
        for (i in 0 until n) {
            ch = s[i]
            if (ch == '(') {
                left++
            } else {
                right++
            }
            if (right > left) {
                left = 0
                right = 0
            }
            if (left == right) {
                max = Math.max(max, left + right)
            }
        }
        left = 0
        right = 0
        for (i in n - 1 downTo 0) {
            ch = s[i]
            if (ch == '(') {
                left++
            } else {
                right++
            }
            if (left > right) {
                left = 0
                right = 0
            }
            if (left == right) {
                max = Math.max(max, left + right)
            }
        }
        return max
    }
}
```