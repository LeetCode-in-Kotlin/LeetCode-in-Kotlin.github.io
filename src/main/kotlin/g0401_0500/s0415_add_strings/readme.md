[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 415\. Add Strings

Easy

Given two non-negative integers, `num1` and `num2` represented as string, return _the sum of_ `num1` _and_ `num2` _as a string_.

You must solve the problem without using any built-in library for handling large integers (such as `BigInteger`). You must also not convert the inputs to integers directly.

**Example 1:**

**Input:** num1 = "11", num2 = "123"

**Output:** "134"

**Example 2:**

**Input:** num1 = "456", num2 = "77"

**Output:** "533"

**Example 3:**

**Input:** num1 = "0", num2 = "0"

**Output:** "0"

**Constraints:**

*   <code>1 <= num1.length, num2.length <= 10<sup>4</sup></code>
*   `num1` and `num2` consist of only digits.
*   `num1` and `num2` don't have any leading zeros except for the zero itself.

## Solution

```kotlin
class Solution {
    fun addStrings(num1: String, num2: String): String {
        var endNum1 = num1.length - 1
        var endNum2 = num2.length - 1
        val res = StringBuilder()
        var carry = 0
        var sum: Int
        while (endNum1 >= 0 || endNum2 >= 0) {
            val a = if (endNum1 >= 0) num1[endNum1] - '0' else 0
            val b = if (endNum2 >= 0) num2[endNum2] - '0' else 0
            sum = (a + b + carry) % 10
            carry = (a + b + carry) / 10
            res.append(sum)
            endNum1--
            endNum2--
        }
        if (carry != 0) {
            res.append(carry)
        }
        return res.reverse().toString()
    }
}
```