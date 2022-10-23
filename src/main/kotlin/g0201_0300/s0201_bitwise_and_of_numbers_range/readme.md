[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 201\. Bitwise AND of Numbers Range

Medium

Given two integers `left` and `right` that represent the range `[left, right]`, return _the bitwise AND of all numbers in this range, inclusive_.

**Example 1:**

**Input:** left = 5, right = 7

**Output:** 4

**Example 2:**

**Input:** left = 0, right = 0

**Output:** 0

**Example 3:**

**Input:** left = 1, right = 2147483647

**Output:** 0

**Constraints:**

*   <code>0 <= left <= right <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    fun rangeBitwiseAnd(left: Int, right: Int): Int {
        return if (left == right) left else right and MASKS[Integer.numberOfLeadingZeros(left xor right)]
    }

    companion object {
        private val MASKS = intArrayOf(
            0,
            -0x80000000,
            -0x40000000,
            -0x20000000,
            -0x10000000,
            -0x8000000,
            -0x4000000,
            -0x2000000,
            -0x1000000,
            -0x800000,
            -0x400000,
            -0x200000,
            -0x100000,
            -0x80000,
            -0x40000,
            -0x20000,
            -0x10000,
            -0x8000,
            -0x4000,
            -0x2000,
            -0x1000,
            -0x800,
            -0x400,
            -0x200,
            -0x100,
            -0x80,
            -0x40,
            -0x20,
            -0x10,
            -0x8,
            -0x4,
            -0x2
        )
    }
}
```