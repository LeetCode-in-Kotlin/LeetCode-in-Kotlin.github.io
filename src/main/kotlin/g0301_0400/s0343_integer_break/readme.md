[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 343\. Integer Break

Medium

Given an integer `n`, break it into the sum of `k` **positive integers**, where `k >= 2`, and maximize the product of those integers.

Return _the maximum product you can get_.

**Example 1:**

**Input:** n = 2

**Output:** 1

**Explanation:** 2 = 1 + 1, 1 × 1 = 1.

**Example 2:**

**Input:** n = 10

**Output:** 36

**Explanation:** 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.

**Constraints:**

*   `2 <= n <= 58`

## Solution

```kotlin
class Solution {
    fun integerBreak(n: Int): Int {
        if (n <= 1) return 0
        if (n == 2) return 1
        if (n == 3) return 2

        var threes = n / 3

        // if there's just 1 remaining, it's better to use 2 x 2's instead of 3 and 1:
        if (n - threes * 3 == 1) threes--
        val twos = (n - threes * 3) / 2

        val threeProd = Math.pow(3.0, threes.toDouble()).toInt()
        val twoProd = Math.pow(2.0, twos.toDouble()).toInt()
        if (threeProd == 0) return twoProd
        if (twoProd == 0) return threeProd

        return twoProd * threeProd
    }
}
```