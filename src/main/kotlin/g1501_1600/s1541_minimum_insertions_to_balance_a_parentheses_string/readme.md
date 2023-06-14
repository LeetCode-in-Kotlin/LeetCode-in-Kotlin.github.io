[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1541\. Minimum Insertions to Balance a Parentheses String

Medium

Given a parentheses string `s` containing only the characters `'('` and `')'`. A parentheses string is **balanced** if:

*   Any left parenthesis `'('` must have a corresponding two consecutive right parenthesis `'))'`.
*   Left parenthesis `'('` must go before the corresponding two consecutive right parenthesis `'))'`.

In other words, we treat `'('` as an opening parenthesis and `'))'` as a closing parenthesis.

*   For example, `"())"`, `"())(())))"` and `"(())())))"` are balanced, `")()"`, `"()))"` and `"(()))"` are not balanced.

You can insert the characters `'('` and `')'` at any position of the string to balance it if needed.

Return _the minimum number of insertions_ needed to make `s` balanced.

**Example 1:**

**Input:** s = "(()))"

**Output:** 1

**Explanation:** The second '(' has two matching '))', but the first '(' has only ')' matching. We need to to add one more ')' at the end of the string to be "(())))" which is balanced.

**Example 2:**

**Input:** s = "())"

**Output:** 0

**Explanation:** The string is already balanced.

**Example 3:**

**Input:** s = "))())("

**Output:** 3

**Explanation:** Add '(' to match the first '))', Add '))' to match the last '('.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of `'('` and `')'` only.

## Solution

```kotlin
class Solution {
    fun minInsertions(s: String): Int {
        var conClosed = 0
        var opened = 0
        var total = 0
        for (i in 0 until s.length) {
            if (s[i] == ')') {
                conClosed++
                if (conClosed == 2) {
                    conClosed = 0
                    if (opened > 0) {
                        opened--
                    } else {
                        total++
                    }
                }
            } else {
                if (conClosed == 1) {
                    total += if (opened > 0) {
                        opened--
                        1
                    } else {
                        2
                    }
                    conClosed = 0
                }
                opened += 1
            }
        }
        if (conClosed == 1) {
            total += if (opened > 0) {
                opened--
                1
            } else {
                2
            }
        }
        total += opened * 2
        return total
    }
}
```