[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3704\. Count No-Zero Pairs That Sum to N

Hard

A **no-zero** integer is a **positive** integer that **does not contain the digit** 0 in its decimal representation.

Given an integer `n`, count the number of pairs `(a, b)` where:

*   `a` and `b` are **no-zero** integers.
*   `a + b = n`

Return an integer denoting the number of such pairs.

**Example 1:**

**Input:** n = 2

**Output:** 1

**Explanation:**

The only pair is `(1, 1)`.

**Example 2:**

**Input:** n = 3

**Output:** 2

**Explanation:**

The pairs are `(1, 2)` and `(2, 1)`.

**Example 3:**

**Input:** n = 11

**Output:** 8

**Explanation:**

The pairs are `(2, 9)`, `(3, 8)`, `(4, 7)`, `(5, 6)`, `(6, 5)`, `(7, 4)`, `(8, 3)`, and `(9, 2)`. Note that `(1, 10)` and `(10, 1)` do not satisfy the conditions because 10 contains 0 in its decimal representation.

**Constraints:**

*   <code>2 <= n <= 10<sup>15</sup></code>

## Solution

```kotlin
class Solution {
    fun countNoZeroPairs(n: Long): Long {
        var m = 0
        var base: Long = 1
        while (base <= n) {
            m++
            base = base * 10
        }
        val digits = IntArray(m)
        var c = n
        for (i in 0..<m) {
            digits[i] = (c % 10).toInt()
            c = c / 10
        }
        var total: Long = 0
        var extra = longArrayOf(1, 0)
        base = 1
        for (p in 0..<m) {
            val nextExtra = longArrayOf(0, 0)
            for (e in 0..1) {
                for (i in 1..9) {
                    for (j in 1..9) {
                        if ((i + j + e) % 10 == digits[p]) {
                            nextExtra[(i + j + e) / 10] += extra[e]
                        }
                    }
                }
            }
            extra = nextExtra
            base = base * 10
            for (e in 0..1) {
                val left = n / base - e
                if (left < 0) {
                    continue
                }
                if (left == 0L) {
                    total += extra[e]
                } else if (isGood(left)) {
                    total += 2 * extra[e]
                }
            }
        }

        return total
    }

    private fun isGood(num: Long): Boolean {
        var num = num
        while (num > 0) {
            if (num % 10 == 0L) {
                return false
            }
            num = num / 10
        }
        return true
    }
}
```