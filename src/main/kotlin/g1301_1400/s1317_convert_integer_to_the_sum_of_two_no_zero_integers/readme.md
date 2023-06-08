[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1317\. Convert Integer to the Sum of Two No-Zero Integers

Easy

**No-Zero integer** is a positive integer that **does not contain any `0`** in its decimal representation.

Given an integer `n`, return _a list of two integers_ `[A, B]` _where_:

*   `A` and `B` are **No-Zero integers**.
*   `A + B = n`

The test cases are generated so that there is at least one valid solution. If there are many valid solutions you can return any of them.

**Example 1:**

**Input:** n = 2

**Output:** [1,1]

**Explanation:** A = 1, B = 1. A + B = n and both A and B do not contain any 0 in their decimal representation.

**Example 2:**

**Input:** n = 11

**Output:** [2,9]

**Constraints:**

*   <code>2 <= n <= 10<sup>4</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun getNoZeroIntegers(n: Int): IntArray {
        var left = 1
        var right = n - 1
        while (left <= right) {
            if (noZero(left) && noZero(right)) {
                return intArrayOf(left, right)
            } else {
                left++
                right--
            }
        }
        return intArrayOf()
    }

    private fun noZero(num: Int): Boolean {
        var num = num
        while (num != 0) {
            num /= if (num % 10 == 0) {
                return false
            } else {
                10
            }
        }
        return true
    }
}
```