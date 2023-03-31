[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 829\. Consecutive Numbers Sum

Hard

Given an integer `n`, return _the number of ways you can write_ `n` _as the sum of consecutive positive integers._

**Example 1:**

**Input:** n = 5

**Output:** 2

**Explanation:** 5 = 2 + 3

**Example 2:**

**Input:** n = 9

**Output:** 3

**Explanation:** 9 = 4 + 5 = 2 + 3 + 4

**Example 3:**

**Input:** n = 15

**Output:** 4

**Explanation:** 15 = 8 + 7 = 4 + 5 + 6 = 1 + 2 + 3 + 4 + 5

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun consecutiveNumbersSum(n: Int): Int {
        var ans = 0
        var i = 1
        while (i * i <= n) {
            if (n % i > 0) {
                i++
                continue
            }
            val j = n / i
            if (i % 2 == 1) {
                ans++
            }
            if (j != i && j % 2 == 1) {
                ans++
            }
            i++
        }
        return ans
    }
}
```