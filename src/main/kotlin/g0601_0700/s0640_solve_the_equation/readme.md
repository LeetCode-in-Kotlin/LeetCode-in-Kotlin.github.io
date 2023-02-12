[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 640\. Solve the Equation

Medium

Solve a given equation and return the value of `'x'` in the form of a string `"x=#value"`. The equation contains only `'+'`, `'-'` operation, the variable `'x'` and its coefficient. You should return `"No solution"` if there is no solution for the equation, or `"Infinite solutions"` if there are infinite solutions for the equation.

If there is exactly one solution for the equation, we ensure that the value of `'x'` is an integer.

**Example 1:**

**Input:** equation = "x+5-3+x=6+x-2"

**Output:** "x=2"

**Example 2:**

**Input:** equation = "x=x"

**Output:** "Infinite solutions"

**Example 3:**

**Input:** equation = "2x=x"

**Output:** "x=0"

**Constraints:**

*   `3 <= equation.length <= 1000`
*   `equation` has exactly one `'='`.
*   `equation` consists of integers with an absolute value in the range `[0, 100]` without any leading zeros, and the variable `'x'`.

## Solution

```kotlin
class Solution {
    fun solveEquation(equation: String): String {
        val eqs = equation.split("=".toRegex()).dropLastWhile { it.isEmpty() }.toTypedArray()
        val arr1 = evaluate(eqs[0])
        val arr2 = evaluate(eqs[1])
        return if (arr1[0] == arr2[0] && arr1[1] == arr2[1]) {
            "Infinite solutions"
        } else if (arr1[0] == arr2[0]) {
            "No solution"
        } else {
            "x=" + (arr2[1] - arr1[1]) / (arr1[0] - arr2[0])
        }
    }

    private fun evaluate(eq: String): IntArray {
        val arr = eq.toCharArray()
        var f = false
        var a = 0
        var b = 0
        var i = 0
        if (arr[0] == '-') {
            f = true
            i++
        }
        while (i < arr.size) {
            if (arr[i] == '-') {
                f = true
                i++
            } else if (arr[i] == '+') {
                i++
            }
            val sb = StringBuilder()
            while (i < arr.size && Character.isDigit(arr[i])) {
                sb.append(arr[i])
                i++
            }
            val n = sb.toString()
            if (i < arr.size && arr[i] == 'x') {
                var number: Int
                number = if (n == "") {
                    1
                } else {
                    n.toInt()
                }
                if (f) {
                    number = -number
                }
                a += number
                i++
            } else {
                var number = n.toInt()
                if (f) {
                    number = -number
                }
                b += number
            }
            f = false
        }
        val op = IntArray(2)
        op[0] = a
        op[1] = b
        return op
    }
}
```