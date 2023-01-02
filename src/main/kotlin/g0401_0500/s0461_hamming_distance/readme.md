[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 461\. Hamming Distance

Easy

The [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) between two integers is the number of positions at which the corresponding bits are different.

Given two integers `x` and `y`, return _the **Hamming distance** between them_.

**Example 1:**

**Input:** x = 1, y = 4

**Output:** 2

**Explanation:** 1 (0 0 0 1) 4 (0 1 0 0) ↑ ↑ The above arrows point to positions where the corresponding bits are different.

**Example 2:**

**Input:** x = 3, y = 1

**Output:** 1

**Constraints:**

*   <code>0 <= x, y <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    fun hammingDistance(x: Int, y: Int): Int {
        return Integer.bitCount(x xor y)
    }
}
```