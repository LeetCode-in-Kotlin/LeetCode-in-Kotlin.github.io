[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 69\. Sqrt(x)

Easy

Given a non-negative integer `x`, compute and return _the square root of_ `x`.

Since the return type is an integer, the decimal digits are **truncated**, and only **the integer part** of the result is returned.

**Note:** You are not allowed to use any built-in exponent function or operator, such as `pow(x, 0.5)` or <code>x ** 0.5</code>.

**Example 1:**

**Input:** x = 4

**Output:** 2

**Example 2:**

**Input:** x = 8

**Output:** 2

**Explanation:** The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.

**Constraints:**

*   <code>0 <= x <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    fun mySqrt(x: Int): Int {
        var start = 1
        var end = x / 2
        var sqrt = start + (end - start) / 2
        if (x == 0) {
            return 0
        }
        while (start <= end) {
            if (sqrt == x / sqrt) {
                return sqrt
            } else if (sqrt > x / sqrt) {
                end = sqrt - 1
            } else if (sqrt < x / sqrt) {
                start = sqrt + 1
            }
            sqrt = start + (end - start) / 2
        }
        return if (sqrt > x / sqrt) {
            sqrt - 1
        } else {
            sqrt
        }
    }
}
```