[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 440\. K-th Smallest in Lexicographical Order

Hard

Given two integers `n` and `k`, return _the_ <code>k<sup>th</sup></code> _lexicographically smallest integer in the range_ `[1, n]`.

**Example 1:**

**Input:** n = 13, k = 2

**Output:** 10

**Explanation:** The lexicographical order is [1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9], so the second smallest number is 10.

**Example 2:**

**Input:** n = 1, k = 1

**Output:** 1

**Constraints:**

*   <code>1 <= k <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun findKthNumber(n: Int, k: Int): Int {
        var k = k
        var curr = 1
        k = k - 1
        while (k > 0) {
            val steps = calSteps(n, curr.toLong(), curr + 1L)
            if (steps <= k) {
                curr += 1
                k -= steps
            } else {
                curr *= 10
                k -= 1
            }
        }
        return curr
    }

    // use long in case of overflow
    private fun calSteps(n: Int, n1: Long, n2: Long): Int {
        var n1 = n1
        var n2 = n2
        var steps: Long = 0
        while (n1 <= n) {
            steps += Math.min(n + 1L, n2) - n1
            n1 *= 10
            n2 *= 10
        }
        return steps.toInt()
    }
}
```