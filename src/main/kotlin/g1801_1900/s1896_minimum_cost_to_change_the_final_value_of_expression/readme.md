[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1896\. Minimum Cost to Change the Final Value of Expression

Hard

You are given a **valid** boolean expression as a string `expression` consisting of the characters `'1'`,`'0'`,`'&'` (bitwise **AND** operator),`'|'` (bitwise **OR** operator),`'('`, and `')'`.

*   For example, `"()1|1"` and `"(1)&()"` are **not valid** while `"1"`, `"(((1))|(0))"`, and `"1|(0&(1))"` are **valid** expressions.

Return _the **minimum cost** to change the final value of the expression_.

*   For example, if `expression = "1|1|(0&0)&1"`, its **value** is `1|1|(0&0)&1 = 1|1|0&1 = 1|0&1 = 1&1 = 1`. We want to apply operations so that the **new** expression evaluates to `0`.

The **cost** of changing the final value of an expression is the **number of operations** performed on the expression. The types of **operations** are described as follows:

*   Turn a `'1'` into a `'0'`.
*   Turn a `'0'` into a `'1'`.
*   Turn a `'&'` into a `'|'`.
*   Turn a `'|'` into a `'&'`.

**Note:** `'&'` does **not** take precedence over `'|'` in the **order of calculation**. Evaluate parentheses **first**, then in **left-to-right** order.

**Example 1:**

**Input:** expression = "1&(0\|1)"

**Output:** 1

**Explanation:** We can turn "1&(0\|1)" into "1&(0&1)" by changing the '\|' to a '&' using 1 operation.

The new expression evaluates to 0. 

**Example 2:**

**Input:** expression = "(0&0)&(0&0&0)"

**Output:** 3

**Explanation:** We can turn "(0&0)&(0&0&0)" into "(0\|1)\|(0&0&0)" using 3 operations.

The new expression evaluates to 1. 

**Example 3:**

**Input:** expression = "(0\|(1\|0&1))"

**Output:** 1

**Explanation:** We can turn "(0\|(1\|0&1))" into "(0\|(0\|0&1))" using 1 operation.

The new expression evaluates to 0.

**Constraints:**

*   <code>1 <= expression.length <= 10<sup>5</sup></code>
*   `expression` only contains `'1'`,`'0'`,`'&'`,`'|'`,`'('`, and `')'`
*   All parentheses are properly matched.
*   There will be no empty parentheses (i.e: `"()"` is not a substring of `expression`).

## Solution

```kotlin
class Solution {
    private class Result(var `val`: Int, var minFlips: Int)

    private var cur = 0

    fun minOperationsToFlip(expression: String): Int {
        cur = 0
        return term(expression).minFlips
    }

    private fun term(s: String): Result {
        var res = factor(s)
        while (cur < s.length && (s[cur] == '|' || s[cur] == '&')) {
            val c = s[cur]
            cur++
            res = if (c == '|') {
                or(res, factor(s))
            } else {
                and(res, factor(s))
            }
        }
        return res
    }

    private fun factor(s: String): Result {
        if (s[cur] == '(') {
            cur++
            val res = term(s)
            cur++
            return res
        }
        return number(s)
    }

    private fun number(s: String): Result {
        return if (s[cur] == '1') {
            cur++
            Result(1, 1)
        } else {
            cur++
            Result(0, 1)
        }
    }

    private fun or(res1: Result, res2: Result): Result {
        return if (res1.`val` + res2.`val` == 0) {
            Result(
                0,
                Math.min(res1.minFlips, res2.minFlips)
            )
        } else if (res1.`val` + res2.`val` == 2) {
            Result(
                1,
                1 + Math.min(res1.minFlips, res2.minFlips)
            )
        } else {
            Result(1, 1)
        }
    }

    private fun and(res1: Result, res2: Result): Result {
        return if (res1.`val` + res2.`val` == 0) {
            Result(
                0,
                1 + Math.min(res1.minFlips, res2.minFlips)
            )
        } else if (res1.`val` + res2.`val` == 2) {
            Result(
                1,
                Math.min(res1.minFlips, res2.minFlips)
            )
        } else {
            Result(0, 1)
        }
    }
}
```