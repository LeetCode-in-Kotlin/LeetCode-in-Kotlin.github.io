[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 241\. Different Ways to Add Parentheses

Medium

Given a string `expression` of numbers and operators, return _all possible results from computing all the different possible ways to group numbers and operators_. You may return the answer in **any order**.

**Example 1:**

**Input:** expression = "2-1-1"

**Output:** [0,2]

**Explanation:** ((2-1)-1) = 0 (2-(1-1)) = 2

**Example 2:**

**Input:** expression = "2\*3-4\*5"

**Output:** [-34,-14,-10,-10,10]

**Explanation:**

    (2*(3-(4*5))) = -34
    ((2*3)-(4*5)) = -14
    ((2*(3-4))*5) = -10
    (2*((3-4)*5)) = -10
    (((2*3)-4)*5) = 10 

**Constraints:**

*   `1 <= expression.length <= 20`
*   `expression` consists of digits and the operator `'+'`, `'-'`, and `'*'`.
*   All the integer values in the input expression are in the range `[0, 99]`.

## Solution

```kotlin
class Solution {
    fun diffWaysToCompute(expression: String): List<Int> {
        return diffWayToCompute(expression, hashMapOf())
    }

    private fun diffWayToCompute(expression: String, hashMap: HashMap<String, List<Int>>): List<Int> {
        if (hashMap.containsKey(expression)) {
            return hashMap.getValue(expression)
        }
        val newList = arrayListOf<Int>()
        if (!hasOperatorInBetween(expression)) {
            newList.add(Integer.parseInt(expression))
        } else {
            for ((index, _) in expression.withIndex()) {
                val operator = expression[index]
                if (!Character.isDigit(operator)) {
                    val leftExpression = diffWayToCompute(expression.substring(0, index), hashMap)
                    val rightExpression = diffWayToCompute(expression.substring(index + 1), hashMap)
                    for (l in leftExpression) {
                        for (r in rightExpression) {
                            when (operator) {
                                '+' -> newList.add(l + r)
                                '-' -> newList.add(l - r)
                                '*' -> newList.add(l * r)
                            }
                        }
                    }
                }
            }
        }
        hashMap[expression] = newList
        return newList
    }

    private fun hasOperatorInBetween(expression: String): Boolean {
        for ((index, _) in expression.withIndex()) {
            when (expression[index]) {
                '+' -> return true
                '-' -> return true
                '*' -> return true
            }
        }
        return false
    }
}
```