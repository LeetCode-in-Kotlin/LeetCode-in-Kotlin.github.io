[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3747\. Count Distinct Integers After Removing Zeros

Medium

You are given a **positive** integer `n`.

For every integer `x` from 1 to `n`, we write down the integer obtained by removing all zeros from the decimal representation of `x`.

Return an integer denoting the number of **distinct** integers written down.

**Example 1:**

**Input:** n = 10

**Output:** 9

**Explanation:**

The integers we wrote down are 1, 2, 3, 4, 5, 6, 7, 8, 9, 1. There are 9 distinct integers (1, 2, 3, 4, 5, 6, 7, 8, 9).

**Example 2:**

**Input:** n = 3

**Output:** 3

**Explanation:**

The integers we wrote down are 1, 2, 3. There are 3 distinct integers (1, 2, 3).

**Constraints:**

*   <code>1 <= n <= 10<sup>15</sup></code>

## Solution

```kotlin
class Solution {
    fun countDistinct(n: Long): Long {
        val digits = n.toString()
        val m = digits.length
        val power9 = LongArray(m + 1)
        power9[0] = 1
        for (i in 1..m) {
            power9[i] = power9[i - 1] * 9
        }
        var total: Long = 0
        for (length in 1..<m) {
            total += power9[length]
        }
        for (idx in 0..<m) {
            val d = digits[idx].code - '0'.code
            if (d == 0) {
                return total
            }
            total += (d - 1) * power9[m - idx - 1]
        }
        return total + 1
    }
}
```