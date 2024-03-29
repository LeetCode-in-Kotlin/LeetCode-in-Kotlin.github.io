[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 736\. Parse Lisp Expression

Hard

You are given a string expression representing a Lisp-like expression to return the integer value of.

The syntax for these expressions is given as follows.

*   An expression is either an integer, let expression, add expression, mult expression, or an assigned variable. Expressions always evaluate to a single integer.
*   (An integer could be positive or negative.)
*   A let expression takes the form <code>"(let v<sub>1</sub> e<sub>1</sub> v<sub>2</sub> e<sub>2</sub> ... v<sub>n</sub> e<sub>n</sub> expr)"</code>, where let is always the string `"let"`, then there are one or more pairs of alternating variables and expressions, meaning that the first variable <code>v<sub>1</sub></code> is assigned the value of the expression <code>e<sub>1</sub></code>, the second variable <code>v<sub>2</sub></code> is assigned the value of the expression <code>e<sub>2</sub></code>, and so on sequentially; and then the value of this let expression is the value of the expression `expr`.
*   An add expression takes the form <code>"(add e<sub>1</sub> e<sub>2</sub>)"</code> where add is always the string `"add"`, there are always two expressions <code>e<sub>1</sub></code>, <code>e<sub>2</sub></code> and the result is the addition of the evaluation of <code>e<sub>1</sub></code> and the evaluation of <code>e<sub>2</sub></code>.
*   A mult expression takes the form <code>"(mult e<sub>1</sub> e<sub>2</sub>)"</code> where mult is always the string `"mult"`, there are always two expressions <code>e<sub>1</sub></code>, <code>e<sub>2</sub></code> and the result is the multiplication of the evaluation of e1 and the evaluation of e2.
*   For this question, we will use a smaller subset of variable names. A variable starts with a lowercase letter, then zero or more lowercase letters or digits. Additionally, for your convenience, the names `"add"`, `"let"`, and `"mult"` are protected and will never be used as variable names.
*   Finally, there is the concept of scope. When an expression of a variable name is evaluated, within the context of that evaluation, the innermost scope (in terms of parentheses) is checked first for the value of that variable, and then outer scopes are checked sequentially. It is guaranteed that every expression is legal. Please see the examples for more details on the scope.

**Example 1:**

**Input:** expression = "(let x 2 (mult x (let x 3 y 4 (add x y))))"

**Output:** 14

**Explanation:** In the expression (add x y), when checking for the value of the variable x, we check from the innermost scope to the outermost in the context of the variable we are trying to evaluate. Since x = 3 is found first, the value of x is 3.

**Example 2:**

**Input:** expression = "(let x 3 x 2 x)"

**Output:** 2

**Explanation:** Assignment in let statements is processed sequentially.

**Example 3:**

**Input:** expression = "(let x 1 y 2 x (add x y) (add x y))"

**Output:** 5

**Explanation:** The first (add x y) evaluates as 3, and is assigned to x. The second (add x y) evaluates as 3+2 = 5.

**Constraints:**

*   `1 <= expression.length <= 2000`
*   There are no leading or trailing spaces in `expression`.
*   All tokens are separated by a single space in `expression`.
*   The answer and all intermediate calculations of that answer are guaranteed to fit in a **32-bit** integer.
*   The expression is guaranteed to be legal and evaluate to an integer.

## Solution

```kotlin
import java.util.Deque
import java.util.LinkedList

class Solution {
    internal class Exp(from: Exp?) {
        var exps: Deque<Exp> = LinkedList()
        var op: String? = null
        var parent: Exp?

        init {
            parent = from
        }

        fun evaluate(vars: Map<String?, Int>): Int {
            return if (op.equals("add", ignoreCase = true)) {
                exps.pop().evaluate(vars) + exps.pop().evaluate(vars)
            } else if (op.equals("mult", ignoreCase = true)) {
                exps.pop().evaluate(vars) * exps.pop().evaluate(vars)
            } else if (op.equals("let", ignoreCase = true)) {
                val nextVars: MutableMap<String?, Int> = HashMap(vars)
                while (exps.size > 1) {
                    val varName = exps.pop().op
                    val `val` = exps.pop().evaluate(nextVars)
                    nextVars[varName] = `val`
                }
                exps.pop().evaluate(nextVars)
            } else {
                if (Character.isLetter(op!![0])) {
                    vars[op]!!
                } else {
                    op!!.toInt()
                }
            }
        }
    }

    private fun buildTree(exp: String): Exp {
        val root = Exp(null)
        var cur: Exp? = root
        var n = exp.length - 1
        while (n >= 0) {
            val c = exp[n]
            if (c == ')') {
                val next = Exp(cur)
                cur!!.exps.push(next)
                cur = next
            } else if (c == '(') {
                cur!!.op = cur.exps.pop().op
                cur = cur.parent
            } else if (c != ' ') {
                var pre = n
                while (pre >= 0 && exp[pre] != '(' && exp[pre] != ' ') {
                    pre--
                }
                val next = Exp(cur)
                next.op = exp.substring(pre + 1, n + 1)
                cur!!.exps.push(next)
                n = pre + 1
            }
            n--
        }
        return root.exps.pop()
    }

    fun evaluate(exp: String): Int {
        return buildTree(exp).evaluate(HashMap())
    }
}
```