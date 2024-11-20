[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1447\. Simplified Fractions

Medium

Given an integer `n`, return _a list of all **simplified** fractions between_ `0` _and_ `1` _(exclusive) such that the denominator is less-than-or-equal-to_ `n`. You can return the answer in **any order**.

**Example 1:**

**Input:** n = 2

**Output:** ["1/2"]

**Explanation:** "1/2" is the only unique fraction with a denominator less-than-or-equal-to 2.

**Example 2:**

**Input:** n = 3

**Output:** ["1/2","1/3","2/3"]

**Example 3:**

**Input:** n = 4

**Output:** ["1/2","1/3","1/4","2/3","3/4"]

**Explanation:** "2/4" is not a simplified fraction because it can be simplified to "1/2".

**Constraints:**

*   `1 <= n <= 100`

## Solution

```kotlin
class Solution {
    fun simplifiedFractions(n: Int): List<String> {
        val result: MutableList<String> = ArrayList()
        if (n == 1) {
            return result
        }
        val str = StringBuilder()
        for (denom in 2..n) {
            for (num in 1 until denom) {
                if (checkGCD(num, denom) == 1) {
                    result.add(str.append(num).append("/").append(denom).toString())
                }
                str.setLength(0)
            }
        }
        return result
    }

    private fun checkGCD(a: Int, b: Int): Int {
        if (a < b) {
            return checkGCD(b, a)
        }
        return if (a == b || a % b == 0 || b == 1) {
            b
        } else {
            checkGCD(a % b, b)
        }
    }
}
```