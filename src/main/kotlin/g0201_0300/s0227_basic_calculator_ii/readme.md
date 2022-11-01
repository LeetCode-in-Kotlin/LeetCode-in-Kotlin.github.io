[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 227\. Basic Calculator II

Medium

Given a string `s` which represents an expression, _evaluate this expression and return its value_.

The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of <code>[-2<sup>31</sup>, 2<sup>31</sup> - 1]</code>.

**Note:** You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

**Example 1:**

**Input:** s = "3+2\*2"

**Output:** 7

**Example 2:**

**Input:** s = " 3/2 "

**Output:** 1

**Example 3:**

**Input:** s = " 3+5 / 2 "

**Output:** 5

**Constraints:**

*   <code>1 <= s.length <= 3 * 10<sup>5</sup></code>
*   `s` consists of integers and operators `('+', '-', '*', '/')` separated by some number of spaces.
*   `s` represents **a valid expression**.
*   All the integers in the expression are non-negative integers in the range <code>[0, 2<sup>31</sup> - 1]</code>.
*   The answer is **guaranteed** to fit in a **32-bit integer**.

## Solution

```kotlin
class Solution {
    fun calculate(s: String): Int {
        var sum = 0
        var tempSum = 0
        var num = 0
        var lastSign = '+'
        for (i in 0 until s.length) {
            val c = s[i]
            if (Character.isDigit(c)) {
                num = num * 10 + c.code - '0'.code
            }
            if (i == s.length - 1 || !Character.isDigit(c) && c != ' ') {
                when (lastSign) {
                    '+' -> {
                        sum += tempSum
                        tempSum = num
                    }
                    '-' -> {
                        sum += tempSum
                        tempSum = -num
                    }
                    '*' -> tempSum *= num
                    '/' -> if (num != 0) {
                        tempSum /= num
                    }
                }
                lastSign = c
                num = 0
            }
        }
        sum += tempSum
        return sum
    }
}
```