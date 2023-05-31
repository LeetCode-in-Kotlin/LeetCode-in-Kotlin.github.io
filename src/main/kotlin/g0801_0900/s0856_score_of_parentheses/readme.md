[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 856\. Score of Parentheses

Medium

Given a balanced parentheses string `s`, return _the **score** of the string_.

The **score** of a balanced parentheses string is based on the following rule:

*   `"()"` has score `1`.
*   `AB` has score `A + B`, where `A` and `B` are balanced parentheses strings.
*   `(A)` has score `2 * A`, where `A` is a balanced parentheses string.

**Example 1:**

**Input:** s = "()"

**Output:** 1

**Example 2:**

**Input:** s = "(())"

**Output:** 2

**Example 3:**

**Input:** s = "()()"

**Output:** 2

**Constraints:**

*   `2 <= s.length <= 50`
*   `s` consists of only `'('` and `')'`.
*   `s` is a balanced parentheses string.

## Solution

```kotlin
import java.util.Deque
import java.util.LinkedList

class Solution {
    fun scoreOfParentheses(s: String): Int {
        val stack: Deque<Int> = LinkedList()
        for (element in s) {
            if (element == '(') {
                stack.push(-1)
            } else {
                var curr = 0
                while (stack.peek() != -1) {
                    curr += stack.pop()
                }
                stack.pop()
                stack.push(if (curr == 0) 1 else curr * 2)
            }
        }
        var score = 0
        while (stack.isNotEmpty()) {
            score += stack.pop()
        }
        return score
    }
}
```