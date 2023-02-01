[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 592\. Fraction Addition and Subtraction

Medium

Given a string `expression` representing an expression of fraction addition and subtraction, return the calculation result in string format.

The final result should be an [irreducible fraction](https://en.wikipedia.org/wiki/Irreducible_fraction). If your final result is an integer, change it to the format of a fraction that has a denominator `1`. So in this case, `2` should be converted to `2/1`.

**Example 1:**

**Input:** expression = "-1/2+1/2"

**Output:** "0/1"

**Example 2:**

**Input:** expression = "-1/2+1/2+1/3"

**Output:** "1/3"

**Example 3:**

**Input:** expression = "1/3-1/2"

**Output:** "-1/6"

**Constraints:**

*   The input string only contains `'0'` to `'9'`, `'/'`, `'+'` and `'-'`. So does the output.
*   Each fraction (input and output) has the format `±numerator/denominator`. If the first input fraction or the output is positive, then `'+'` will be omitted.
*   The input only contains valid **irreducible fractions**, where the **numerator** and **denominator** of each fraction will always be in the range `[1, 10]`. If the denominator is `1`, it means this fraction is actually an integer in a fraction format defined above.
*   The number of given fractions will be in the range `[1, 10]`.
*   The numerator and denominator of the **final result** are guaranteed to be valid and in the range of **32-bit** int.

## Solution

```kotlin
class Solution {
    private fun gcd(a: Int, b: Int): Int {
        return if (a % b == 0) b else gcd(b, a % b)
    }

    private fun format(a: Int, b: Int): String {
        val gcd = Math.abs(gcd(a, b))
        return (a / gcd).toString() + "/" + b / gcd
    }

    private fun parse(s: String): IntArray {
        val idx = s.indexOf("/")
        return intArrayOf(s.substring(0, idx).toInt(), s.substring(idx + 1).toInt())
    }

    fun fractionAddition(expression: String): String {
        var rst = intArrayOf(0, 1)
        val list: MutableList<IntArray> = ArrayList()
        var sb = StringBuilder().append(expression[0])
        for (i in 1 until expression.length) {
            val c = expression[i]
            if (c == '+' || c == '-') {
                list.add(parse(sb.toString()))
                sb = StringBuilder().append(c)
            } else {
                sb.append(c)
            }
        }
        list.add(parse(sb.toString()))
        for (num in list) {
            rst = intArrayOf(rst[0] * num[1] + rst[1] * num[0], rst[1] * num[1])
        }
        return format(rst[0], rst[1])
    }
}
```