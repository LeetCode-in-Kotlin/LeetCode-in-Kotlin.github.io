[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 224\. Basic Calculator

Hard

Given a string `s` representing a valid expression, implement a basic calculator to evaluate it, and return _the result of the evaluation_.

**Note:** You are **not** allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

**Example 1:**

**Input:** s = "1 + 1"

**Output:** 2

**Example 2:**

**Input:** s = " 2-1 + 2 "

**Output:** 3

**Example 3:**

**Input:** s = "(1+(4+5+2)-3)+(6+8)"

**Output:** 23

**Constraints:**

*   <code>1 <= s.length <= 3 * 10<sup>5</sup></code>
*   `s` consists of digits, `'+'`, `'-'`, `'('`, `')'`, and `' '`.
*   `s` represents a valid expression.
*   `'+'` is **not** used as a unary operation (i.e., `"+1"` and `"+(2 + 3)"` is invalid).
*   `'-'` could be used as a unary operation (i.e., `"-1"` and `"-(2 + 3)"` is valid).
*   There will be no two consecutive operators in the input.
*   Every number and running calculation will fit in a signed 32-bit integer.

## Solution

```kotlin
class Solution {
    private var i = 0
    fun calculate(s: String): Int {
        val ca = s.toCharArray()
        return helper(ca)
    }

    private fun helper(ca: CharArray): Int {
        var num = 0
        var prenum = 0
        var isPlus = true
        while (i < ca.size) {
            val c = ca[i]
            if (c != ' ') {
                if (c >= '0' && c <= '9') {
                    num = if (num == 0) {
                        c.code - '0'.code
                    } else {
                        num * 10 + c.code - '0'.code
                    }
                } else if (c == '+') {
                    prenum += num * if (isPlus) 1 else -1
                    isPlus = true
                    num = 0
                } else if (c == '-') {
                    prenum += num * if (isPlus) 1 else -1
                    num = 0
                    isPlus = false
                } else if (c == '(') {
                    i++
                    prenum += helper(ca) * if (isPlus) 1 else -1
                    isPlus = true
                    num = 0
                } else if (c == ')') {
                    return prenum + num * if (isPlus) 1 else -1
                }
            }
            i++
        }
        return prenum + num * if (isPlus) 1 else -1
    }
}
```