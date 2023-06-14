[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1486\. XOR Operation in an Array

Easy

You are given an integer `n` and an integer `start`.

Define an array `nums` where `nums[i] = start + 2 * i` (**0-indexed**) and `n == nums.length`.

Return _the bitwise XOR of all elements of_ `nums`.

**Example 1:**

**Input:** n = 5, start = 0

**Output:** 8

**Explanation:** Array nums is equal to [0, 2, 4, 6, 8] where (0 ^ 2 ^ 4 ^ 6 ^ 8) = 8. Where "^" corresponds to bitwise XOR operator.

**Example 2:**

**Input:** n = 4, start = 3

**Output:** 8

**Explanation:** Array nums is equal to [3, 5, 7, 9] where (3 ^ 5 ^ 7 ^ 9) = 8.

**Constraints:**

*   `1 <= n <= 1000`
*   `0 <= start <= 1000`
*   `n == nums.length`

## Solution

```kotlin
class Solution {
    fun xorOperation(n: Int, start: Int): Int {
        val nums = IntArray(n)
        for (i in 0 until n) {
            nums[i] = start + 2 * i
        }
        var result = 0
        for (num in nums) {
            result = result xor num
        }
        return result
    }
}
```