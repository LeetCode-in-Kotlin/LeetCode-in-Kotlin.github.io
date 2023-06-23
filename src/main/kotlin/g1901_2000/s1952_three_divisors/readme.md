[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1952\. Three Divisors

Easy

Given an integer `n`, return `true` _if_ `n` _has **exactly three positive divisors**. Otherwise, return_ `false`.

An integer `m` is a **divisor** of `n` if there exists an integer `k` such that `n = k * m`.

**Example 1:**

**Input:** n = 2

**Output:** false

**Explantion:** 2 has only two divisors: 1 and 2.

**Example 2:**

**Input:** n = 4

**Output:** true

**Explantion:** 4 has three divisors: 1, 2, and 4.

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun isThree(n: Int): Boolean {
        var divisors = 0
        for (i in 1..n) {
            if (n % i == 0) {
                divisors++
            }
        }
        return divisors == 3
    }
}
```