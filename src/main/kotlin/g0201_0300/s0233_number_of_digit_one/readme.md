[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 233\. Number of Digit One

Hard

Given an integer `n`, count _the total number of digit_ `1` _appearing in all non-negative integers less than or equal to_ `n`.

**Example 1:**

**Input:** n = 13

**Output:** 6

**Example 2:**

**Input:** n = 0

**Output:** 0

**Constraints:**

*   <code>0 <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun countDigitOne(n: Int): Int {
        var ans = 0
        var k = n
        var cum = 0
        var curr10 = 1
        while (k > 0) {
            val rem = k % 10
            val q = k / 10
            ans += if (rem == 0) {
                q * curr10
            } else if (rem == 1) {
                q * curr10 + cum + 1
            } else {
                (q + 1) * curr10
            }
            k = q
            cum += rem * curr10
            curr10 *= 10
        }
        return ans
    }
}
```