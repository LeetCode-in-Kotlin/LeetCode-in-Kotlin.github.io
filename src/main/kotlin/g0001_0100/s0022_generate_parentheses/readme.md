[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 22\. Generate Parentheses

Medium

Given `n` pairs of parentheses, write a function to _generate all combinations of well-formed parentheses_.

**Example 1:**

**Input:** n = 3

**Output:** ["((()))","(()())","(())()","()(())","()()()"]

**Example 2:**

**Input:** n = 1

**Output:** ["()"]

**Constraints:**

*   `1 <= n <= 8`

## Solution

```kotlin
class Solution {
    fun generateParenthesis(n: Int): List<String> {
        val sb = StringBuilder()
        val ans: MutableList<String> = ArrayList()
        return generate(sb, ans, n, n)
    }

    private fun generate(sb: StringBuilder, str: MutableList<String>, open: Int, close: Int): List<String> {
        if (open == 0 && close == 0) {
            str.add(sb.toString())
            return str
        }
        if (open > 0) {
            sb.append('(')
            generate(sb, str, open - 1, close)
            sb.deleteCharAt(sb.length - 1)
        }
        if (close > 0 && open < close) {
            sb.append(')')
            generate(sb, str, open, close - 1)
            sb.deleteCharAt(sb.length - 1)
        }
        return str
    }
}
```