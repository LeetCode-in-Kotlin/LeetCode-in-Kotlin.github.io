[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1780\. Check if Number is a Sum of Powers of Three

Medium

Given an integer `n`, return `true` _if it is possible to represent_ `n` _as the sum of distinct powers of three._ Otherwise, return `false`.

An integer `y` is a power of three if there exists an integer `x` such that <code>y == 3<sup>x</sup></code>.

**Example 1:**

**Input:** n = 12

**Output:** true

**Explanation:** 12 = 3<sup>1</sup> + 3<sup>2</sup>

**Example 2:**

**Input:** n = 91

**Output:** true

**Explanation:** 91 = 3<sup>0</sup> + 3<sup>2</sup> + 3<sup>4</sup>

**Example 3:**

**Input:** n = 21

**Output:** false

**Constraints:**

*   <code>1 <= n <= 10<sup>7</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun checkPowersOfThree(n: Int): Boolean {
        var n = n
        while (n != 0) {
            val rem = n % 3
            n /= 3
            if (rem == 2 || n == 2) {
                return false
            }
        }
        return true
    }
}
```