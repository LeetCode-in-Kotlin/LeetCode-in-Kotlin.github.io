[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 796\. Rotate String

Easy

Given two strings `s` and `goal`, return `true` _if and only if_ `s` _can become_ `goal` _after some number of **shifts** on_ `s`.

A **shift** on `s` consists of moving the leftmost character of `s` to the rightmost position.

*   For example, if `s = "abcde"`, then it will be `"bcdea"` after one shift.

**Example 1:**

**Input:** s = "abcde", goal = "cdeab"

**Output:** true

**Example 2:**

**Input:** s = "abcde", goal = "abced"

**Output:** false

**Constraints:**

*   `1 <= s.length, goal.length <= 100`
*   `s` and `goal` consist of lowercase English letters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private fun check(s: String, goal: String, i: Int): Boolean {
        var i = i
        var j = 0
        val len = goal.length
        while (j < len) {
            if (i == len) {
                i = 0
            }
            if (s[i] != goal[j]) {
                return false
            }
            j++
            i++
        }
        return true
    }

    fun rotateString(s: String, goal: String): Boolean {
        if (s.length != goal.length) {
            return false
        }
        val len = s.length
        if (s[0] == goal[0] && s != goal) {
            return false
        }
        for (i in 0 until len) {
            if (s[i] == goal[0] && check(s, goal, i)) {
                return true
            }
        }
        return false
    }
}
```