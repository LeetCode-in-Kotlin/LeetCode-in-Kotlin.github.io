[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3941\. Password Strength

Medium

You are given a string `password`.

The **strength** of the password is calculated based on the following rules:

*   1 point for each distinct lowercase letter (`'a'` to `'z'`).
*   2 points for each distinct uppercase letter (`'A'` to `'Z'`).
*   3 points for each distinct digit (`'0'` to `'9'`).
*   5 points for each distinct special character from the set `"!@#$"`.

Each character contributes **at most** once, even if it appears multiple times.

Return an integer denoting the strength of the password.

**Example 1:**

**Input:** password = "aA1!"

**Output:** 11

**Explanation:**

*   The distinct characters are `'a'`, `'A'`, `'1'` and `'!'`.
*   Thus, the `strength = 1 + 2 + 3 + 5 = 11`.

**Example 2:**

**Input:** password = "bbB11#"

**Output:** 11

**Explanation:**

*   The distinct characters are `'b'`, `'B'`, `'1'` and `'#'`.
*   Thus, the `strength = 1 + 2 + 3 + 5 = 11`.

**Constraints:**

*   <code>1 <= password.length <= 10<sup>5</sup></code>
*   `password` consists of lowercase and uppercase English letters, digits, and special characters from `"!@#$"`.

## Solution

```kotlin
class Solution {
    fun passwordStrength(password: String): Int {
        var strength = 0
        val present = BooleanArray(256)
        for (i in 0..<password.length) {
            present[password[i].code] = true
        }
        for (i in 97..122) {
            if (present[i]) {
                strength++
            }
        }
        for (i in 65..90) {
            if (present[i]) {
                strength += 2
            }
        }
        for (i in 48..57) {
            if (present[i]) {
                strength += 3
            }
        }
        if (present['!'.code]) {
            strength += 5
        }
        if (present['@'.code]) {
            strength += 5
        }
        if (present['#'.code]) {
            strength += 5
        }
        if (present['$'.code]) {
            strength += 5
        }
        return strength
    }
}
```