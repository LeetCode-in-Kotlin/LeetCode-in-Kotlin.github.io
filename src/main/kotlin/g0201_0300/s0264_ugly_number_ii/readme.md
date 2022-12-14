[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 264\. Ugly Number II

Medium

An **ugly number** is a positive integer whose prime factors are limited to `2`, `3`, and `5`.

Given an integer `n`, return _the_ <code>n<sup>th</sup></code> _**ugly number**_.

**Example 1:**

**Input:** n = 10

**Output:** 12

**Explanation:** [1, 2, 3, 4, 5, 6, 8, 9, 10, 12] is the sequence of the first 10 ugly numbers.

**Example 2:**

**Input:** n = 1

**Output:** 1

**Explanation:** 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.

**Constraints:**

*   `1 <= n <= 1690`

## Solution

```kotlin
class Solution {
    fun nthUglyNumber(n: Int): Int {
        val ugly = IntArray(n)
        var i2 = 0
        var i3 = 0
        var i5 = 0
        var ugly2 = 2
        var ugly3 = 3
        var ugly5 = 5
        var nextugly: Int
        ugly[0] = 1
        for (i in 1 until n) {
            nextugly = Math.min(Math.min(ugly2, ugly3), ugly5)
            ugly[i] = nextugly
            if (nextugly == ugly2) {
                i2++
                ugly2 = ugly[i2] * 2
            }
            if (nextugly == ugly3) {
                i3++
                ugly3 = ugly[i3] * 3
            }
            if (nextugly == ugly5) {
                i5++
                ugly5 = ugly[i5] * 5
            }
        }
        return ugly[n - 1]
    }
}
```