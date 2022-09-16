[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 7\. Reverse Integer

Medium

Given a signed 32-bit integer `x`, return `x` _with its digits reversed_. If reversing `x` causes the value to go outside the signed 32-bit integer range <code>[-2<sup>31</sup>, 2<sup>31</sup> - 1]</code>, then return `0`.

**Assume the environment does not allow you to store 64-bit integers (signed or unsigned).**

**Example 1:**

**Input:** x = 123

**Output:** 321 

**Example 2:**

**Input:** x = -123

**Output:** -321 

**Example 3:**

**Input:** x = 120

**Output:** 21 

**Example 4:**

**Input:** x = 0

**Output:** 0 

**Constraints:**

*   <code>-2<sup>31</sup> <= x <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    fun reverse(x: Int): Int {
        var rev: Long = 0
        var localX = x
        while (localX != 0) {
            rev = (rev * 10) + (localX % 10)
            localX /= 10
        }
        if (rev > Integer.MAX_VALUE || rev < Integer.MIN_VALUE) {
            return 0
        }
        return rev.toInt()
    }
}
```