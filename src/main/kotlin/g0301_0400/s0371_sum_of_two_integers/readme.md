[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 371\. Sum of Two Integers

Medium

Given two integers `a` and `b`, return _the sum of the two integers without using the operators_ `+` _and_ `-`.

**Example 1:**

**Input:** a = 1, b = 2

**Output:** 3

**Example 2:**

**Input:** a = 2, b = 3

**Output:** 5

**Constraints:**

*   `-1000 <= a, b <= 1000`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun getSum(a: Int, b: Int): Int {
        var a = a
        var b = b
        var ans = 0
        var memo = 0
        var exp = 0
        var count = 0
        while (count < 32) {
            val val1 = a and 1
            val val2 = b and 1
            var `val` = sum(val1, val2, memo)
            memo = `val` shr 1
            `val` = `val` and 1
            a = a shr 1
            b = b shr 1
            ans = ans or (`val` shl exp)
            exp = plusOne(exp)
            count = plusOne(count)
        }
        return ans
    }

    private fun sum(val1: Int, val2: Int, val3: Int): Int {
        var count = 0
        if (val1 == 1) {
            count = plusOne(count)
        }
        if (val2 == 1) {
            count = plusOne(count)
        }
        if (val3 == 1) {
            count = plusOne(count)
        }
        return count
    }

    private fun plusOne(`val`: Int): Int {
        return -`val`.inv()
    }
}
```