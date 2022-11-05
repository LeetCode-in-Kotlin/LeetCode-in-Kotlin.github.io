[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 263\. Ugly Number

Easy

An **ugly number** is a positive integer whose prime factors are limited to `2`, `3`, and `5`.

Given an integer `n`, return `true` _if_ `n` _is an **ugly number**_.

**Example 1:**

**Input:** n = 6

**Output:** true

**Explanation:** 6 = 2 × 3

**Example 2:**

**Input:** n = 1

**Output:** true

**Explanation:** 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.

**Example 3:**

**Input:** n = 14

**Output:** false

**Explanation:** 14 is not ugly since it includes the prime factor 7.

**Constraints:**

*   <code>-2<sup>31</sup> <= n <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    fun isUgly(n: Int): Boolean {
        if (n < 1) {
            return false
        }
        if (n == 1) {
            return true
        }
        var num = n
        for (divider in arrayOf(2, 3, 5)) {
            while (num % divider == 0) {
                num /= divider
            }
        }
        return num == 1
    }
}
```