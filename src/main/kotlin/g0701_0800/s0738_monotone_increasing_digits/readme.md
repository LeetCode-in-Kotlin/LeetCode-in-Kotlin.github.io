[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 738\. Monotone Increasing Digits

Medium

An integer has **monotone increasing digits** if and only if each pair of adjacent digits `x` and `y` satisfy `x <= y`.

Given an integer `n`, return _the largest number that is less than or equal to_ `n` _with **monotone increasing digits**_.

**Example 1:**

**Input:** n = 10

**Output:** 9

**Example 2:**

**Input:** n = 1234

**Output:** 1234

**Example 3:**

**Input:** n = 332

**Output:** 299

**Constraints:**

*   <code>0 <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun monotoneIncreasingDigits(n: Int): Int {
        var n = n
        var i = 10
        while (n / i > 0) {
            val digit = n / i % 10
            val endNum = n % i
            val firstEndNum = endNum * 10 / i
            if (digit > firstEndNum) {
                n -= endNum + 1
            }
            i *= 10
        }
        return n
    }
}
```