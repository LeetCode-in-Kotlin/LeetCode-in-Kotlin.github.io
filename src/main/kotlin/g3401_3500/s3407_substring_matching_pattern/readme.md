[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3407\. Substring Matching Pattern

Easy

You are given a string `s` and a pattern string `p`, where `p` contains **exactly one** `'*'` character.

The `'*'` in `p` can be replaced with any sequence of zero or more characters.

Return `true` if `p` can be made a substring of `s`, and `false` otherwise.

A **substring** is a contiguous **non-empty** sequence of characters within a string.

**Example 1:**

**Input:** s = "leetcode", p = "ee\*e"

**Output:** true

**Explanation:**

By replacing the `'*'` with `"tcod"`, the substring `"eetcode"` matches the pattern.

**Example 2:**

**Input:** s = "car", p = "c\*v"

**Output:** false

**Explanation:**

There is no substring matching the pattern.

**Example 3:**

**Input:** s = "luck", p = "u\*"

**Output:** true

**Explanation:**

The substrings `"u"`, `"uc"`, and `"uck"` match the pattern.

**Constraints:**

*   `1 <= s.length <= 50`
*   `1 <= p.length <= 50`
*   `s` contains only lowercase English letters.
*   `p` contains only lowercase English letters and exactly one `'*'`

## Solution

```kotlin
class Solution {
    fun hasMatch(s: String, p: String): Boolean {
        var index = -1
        for (i in 0..<p.length) {
            if (p[i] == '*') {
                index = i
                break
            }
        }
        val num1 = `fun`(s, p.substring(0, index))
        if (num1 == -1) {
            return false
        }
        val num2 = `fun`(s.substring(num1), p.substring(index + 1))
        return num2 != -1
    }

    private fun `fun`(s: String, k: String): Int {
        val n = s.length
        val m = k.length
        var j: Int
        for (i in 0..n - m) {
            j = 0
            while (j < m) {
                val ch1 = s[j + i]
                val ch2 = k[j]
                if (ch1 != ch2) {
                    break
                }
                j++
            }
            if (j == m) {
                return i + j
            }
        }
        return -1
    }
}
```