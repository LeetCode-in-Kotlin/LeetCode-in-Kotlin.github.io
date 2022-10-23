[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 202\. Happy Number

Easy

Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

*   Starting with any positive integer, replace the number by the sum of the squares of its digits.
*   Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
*   Those numbers for which this process **ends in 1** are happy.

Return `true` _if_ `n` _is a happy number, and_ `false` _if not_.

**Example 1:**

**Input:** n = 19

**Output:** true

**Explanation:** 1<sup>2</sup> + 9<sup>2</sup> = 82 8<sup>2</sup> + 2<sup>2</sup> = 68 6<sup>2</sup> + 8<sup>2</sup> = 100 1<sup>2</sup> + 0<sup>2</sup> + 0<sup>2</sup> = 1

**Example 2:**

**Input:** n = 2

**Output:** false

**Constraints:**

*   <code>1 <= n <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    private val set = mutableSetOf<Int>()

    tailrec fun isHappy(n: Int): Boolean {
        var num = n
        var squareSum = 0
        while (num > 0) {
            val digit = num % 10
            squareSum += digit * digit
            num /= 10
        }
        if (squareSum == 1) {
            return true
        }
        if (set.contains(squareSum)) {
            return false
        }
        set.add(squareSum)
        return isHappy(squareSum)
    }
}
```