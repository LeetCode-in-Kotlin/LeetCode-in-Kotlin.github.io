[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 150\. Evaluate Reverse Polish Notation

Medium

Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are `+`, `-`, `*`, and `/`. Each operand may be an integer or another expression.

**Note** that division between two integers should truncate toward zero.

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.

**Example 1:**

**Input:** tokens = ["2","1","+","3","\*"]

**Output:** 9

**Explanation:** ((2 + 1) \* 3) = 9

**Example 2:**

**Input:** tokens = ["4","13","5","/","+"]

**Output:** 6

**Explanation:** (4 + (13 / 5)) = 6

**Example 3:**

**Input:** tokens = ["10","6","9","3","+","-11","\*","/","\*","17","+","5","+"]

**Output:** 22

**Explanation:** ((10 \* (6 / ((9 + 3) \* -11))) + 17) + 5 

= ((10 \* (6 / (12 \* -11))) + 17) + 5 

= ((10 \* (6 / -132)) + 17) + 5 

= ((10 \* 0) + 17) + 5 

= (0 + 17) + 5 

= 17 + 5 

= 22

**Constraints:**

*   <code>1 <= tokens.length <= 10<sup>4</sup></code>
*   `tokens[i]` is either an operator: `"+"`, `"-"`, `"*"`, or `"/"`, or an integer in the range `[-200, 200]`.

## Solution

```kotlin
class Solution {
    val op = mapOf<String, (Int, Int) -> Int>(
        "/" to { a, b -> a / b },
        "*" to { a, b -> a * b },
        "+" to { a, b -> a + b },
        "-" to { a, b -> a - b },
    )
    fun evalRPN(tokens: Array<String>): Int {
        val stack = ArrayDeque<String>()
        for (t in tokens) {
            if (op.contains(t)) {
                val b = stack.removeFirst().toInt()
                val a = stack.removeFirst().toInt()
                val c = op.getValue(t).invoke(a, b)
                stack.addFirst(c.toString())
            } else {
                stack.addFirst(t)
            }
        }
        return stack.removeFirst().toInt()
    }
}
```