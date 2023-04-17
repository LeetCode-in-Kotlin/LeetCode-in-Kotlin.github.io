[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 921\. Minimum Add to Make Parentheses Valid

Medium

A parentheses string is valid if and only if:

*   It is the empty string,
*   It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
*   It can be written as `(A)`, where `A` is a valid string.

You are given a parentheses string `s`. In one move, you can insert a parenthesis at any position of the string.

*   For example, if `s = "()))"`, you can insert an opening parenthesis to be <code>"(**(**)))"</code> or a closing parenthesis to be <code>"())**)**)"</code>.

Return _the minimum number of moves required to make_ `s` _valid_.

**Example 1:**

**Input:** s = "())"

**Output:** 1

**Example 2:**

**Input:** s = "((("

**Output:** 3

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s[i]` is either `'('` or `')'`.

## Solution

```kotlin
import java.util.Deque
import java.util.LinkedList

class Solution {
    fun minAddToMakeValid(s: String): Int {
        val stack: Deque<Char> = LinkedList()
        for (c in s.toCharArray()) {
            if (c == ')') {
                if (!stack.isEmpty() && stack.peek() == '(') {
                    stack.pop()
                } else {
                    stack.push(c)
                }
            } else {
                stack.push(c)
            }
        }
        return stack.size
    }
}
```