[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 400\. Nth Digit

Medium

Given an integer `n`, return the <code>n<sup>th</sup></code> digit of the infinite integer sequence `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...]`.

**Example 1:**

**Input:** n = 3

**Output:** 3

**Example 2:**

**Input:** n = 11

**Output:** 0

**Explanation:** The 11<sup>th</sup> digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... is a 0, which is part of the number 10.

**Constraints:**

*   <code>1 <= n <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    /*
    * 1. find the length of the number where the nth digit is from
    * 2. find the actual number where the nth digit is from
    * 3. find the nth digit and return
    */
    fun findNthDigit(n: Int): Int {
        var n = n
        var len = 1
        var count: Long = 9
        var start = 1
        while (n > len * count) {
            n -= (len * count).toInt()
            len += 1
            count *= 10
            start *= 10
        }
        start += (n - 1) / len
        val s = Integer.toString(start)
        return Character.getNumericValue(s[(n - 1) % len])
    }
}
```