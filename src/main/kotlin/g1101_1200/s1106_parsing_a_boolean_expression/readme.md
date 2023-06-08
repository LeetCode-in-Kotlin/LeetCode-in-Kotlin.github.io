[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1106\. Parsing A Boolean Expression

Hard

Return the result of evaluating a given boolean `expression`, represented as a string.

An expression can either be:

*   `"t"`, evaluating to `True`;
*   `"f"`, evaluating to `False`;
*   `"!(expr)"`, evaluating to the logical NOT of the inner expression `expr`;
*   `"&(expr1,expr2,...)"`, evaluating to the logical AND of 2 or more inner expressions `expr1, expr2, ...`;
*   `"|(expr1,expr2,...)"`, evaluating to the logical OR of 2 or more inner expressions `expr1, expr2, ...`

**Example 1:**

**Input:** expression = "!(f)"

**Output:** true

**Example 2:**

**Input:** expression = "\|(f,t)"

**Output:** true

**Example 3:**

**Input:** expression = "&(t,f)"

**Output:** false

**Constraints:**

*   <code>1 <= expression.length <= 2 * 10<sup>4</sup></code>
*   `expression[i]` consists of characters in `{'(', ')', '&', '|', '!', 't', 'f', ','}`.
*   `expression` is a valid expression representing a boolean, as given in the description.

## Solution

```kotlin
class Solution {
    private var source: String? = null
    private var index = 0

    fun parseBoolExpr(expression: String?): Boolean {
        source = expression
        index = 0
        return expr()
    }

    private fun expr(): Boolean {
        val res: Boolean
        res = if (match('!')) {
            not()
        } else if (match('&')) {
            and()
        } else if (match('|')) {
            or()
        } else {
            bool()
        }
        return res
    }

    private fun not(): Boolean {
        consume('!')
        return !group()[0]
    }

    private fun or(): Boolean {
        consume('|')
        var res = false
        for (e in group()) {
            res = res or e
        }
        return res
    }

    private fun and(): Boolean {
        consume('&')
        var res = true
        for (e in group()) {
            res = res and e
        }
        return res
    }

    private fun group(): List<Boolean> {
        consume('(')
        val res: MutableList<Boolean> = ArrayList()
        while (!match(')')) {
            res.add(expr())
            if (match(',')) {
                advance()
            }
        }
        consume(')')
        return res
    }

    private fun bool(): Boolean {
        val isTrue = match('t')
        advance()
        return isTrue
    }

    private val isAtEnd: Boolean
        get() = index >= source!!.length

    private fun advance() {
        if (isAtEnd) {
            return
        }
        index++
    }

    private fun peek(): Char {
        return source!![index]
    }

    private fun match(ch: Char): Boolean {
        return if (isAtEnd) {
            false
        } else peek() == ch
    }

    private fun consume(ch: Char) {
        if (!match(ch)) {
            return
        }
        advance()
    }
}
```