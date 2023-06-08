[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1362\. Closest Divisors

Medium

Given an integer `num`, find the closest two integers in absolute difference whose product equals `num + 1` or `num + 2`.

Return the two integers in any order.

**Example 1:**

**Input:** num = 8

**Output:** [3,3]

**Explanation:** For num + 1 = 9, the closest divisors are 3 & 3, for num + 2 = 10, the closest divisors are 2 & 5, hence 3 & 3 is chosen.

**Example 2:**

**Input:** num = 123

**Output:** [5,25]

**Example 3:**

**Input:** num = 999

**Output:** [40,25]

**Constraints:**

*   `1 <= num <= 10^9`

## Solution

```kotlin
class Solution {
    fun closestDivisors(num: Int): IntArray {
        val sqrt1 = Math.sqrt(num + 1.0).toInt()
        val sqrt2 = Math.sqrt(num + 2.0).toInt()
        if (sqrt1 * sqrt1 == num + 1) {
            return intArrayOf(sqrt1, sqrt1)
        }
        if (sqrt2 * sqrt2 == num + 2) {
            return intArrayOf(sqrt2, sqrt2)
        }
        var ans1 = IntArray(2)
        for (i in sqrt1 downTo 1) {
            if ((num + 1) % i == 0) {
                ans1 = intArrayOf(i, (num + 1) / i)
                break
            }
        }
        var ans2 = IntArray(2)
        for (i in sqrt2 downTo 1) {
            if ((num + 2) % i == 0) {
                ans2 = intArrayOf(i, (num + 2) / i)
                break
            }
        }
        return if (Math.abs(ans2[0] - ans2[1]) < Math.abs(ans1[0] - ans1[1])) ans2 else ans1
    }
}
```