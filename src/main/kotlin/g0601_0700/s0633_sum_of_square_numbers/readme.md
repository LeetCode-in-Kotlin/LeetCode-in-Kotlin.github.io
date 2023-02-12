[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 633\. Sum of Square Numbers

Medium

Given a non-negative integer `c`, decide whether there're two integers `a` and `b` such that <code>a<sup>2</sup> + b<sup>2</sup> = c</code>.

**Example 1:**

**Input:** c = 5

**Output:** true

**Explanation:** 1 \* 1 + 2 \* 2 = 5

**Example 2:**

**Input:** c = 3

**Output:** false

**Constraints:**

*   <code>0 <= c <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
import kotlin.math.sqrt

class Solution {
    fun judgeSquareSum(c: Int): Boolean {
        val right = sqrt(c.toDouble()).toInt()
        val left = sqrt(c.toDouble() / 2).toInt()
        for (i in left..right) {
            val j = sqrt(c - (i * i).toDouble()).toInt()
            if (i * i + j * j == c) {
                return true
            }
        }
        return false
    }
}
```