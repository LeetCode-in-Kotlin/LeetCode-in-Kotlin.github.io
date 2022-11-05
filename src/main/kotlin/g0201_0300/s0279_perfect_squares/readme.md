[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 279\. Perfect Squares

Medium

Given an integer `n`, return _the least number of perfect square numbers that sum to_ `n`.

A **perfect square** is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, `1`, `4`, `9`, and `16` are perfect squares while `3` and `11` are not.

**Example 1:**

**Input:** n = 12

**Output:** 3

**Explanation:** 12 = 4 + 4 + 4.

**Example 2:**

**Input:** n = 13

**Output:** 2

**Explanation:** 13 = 4 + 9.

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private fun validSquare(n: Int): Boolean {
        val root = Math.sqrt(n.toDouble()).toInt()
        return root * root == n
    }

    fun numSquares(n: Int): Int {
        var n = n
        if (n < 4) {
            return n
        }
        if (validSquare(n)) {
            return 1
        }
        while (n and 3 == 0) {
            n = n shr 2
        }
        if (n and 7 == 7) {
            return 4
        }
        val x = Math.sqrt(n.toDouble()).toInt()
        for (i in 1..x) {
            if (validSquare(n - i * i)) {
                return 2
            }
        }
        return 3
    }
}
```