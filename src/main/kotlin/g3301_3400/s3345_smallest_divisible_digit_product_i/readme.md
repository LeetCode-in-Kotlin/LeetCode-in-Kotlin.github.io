[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3345\. Smallest Divisible Digit Product I

Easy

You are given two integers `n` and `t`. Return the **smallest** number greater than or equal to `n` such that the **product of its digits** is divisible by `t`.

**Example 1:**

**Input:** n = 10, t = 2

**Output:** 10

**Explanation:**

The digit product of 10 is 0, which is divisible by 2, making it the smallest number greater than or equal to 10 that satisfies the condition.

**Example 2:**

**Input:** n = 15, t = 3

**Output:** 16

**Explanation:**

The digit product of 16 is 6, which is divisible by 3, making it the smallest number greater than or equal to 15 that satisfies the condition.

**Constraints:**

*   `1 <= n <= 100`
*   `1 <= t <= 10`

## Solution

```kotlin
class Solution {
    fun smallestNumber(n: Int, t: Int): Int {
        var num = -1
        var check = n
        while (num == -1) {
            val product = findProduct(check)
            if (product % t == 0) {
                num = check
            }
            check += 1
        }
        return num
    }

    private fun findProduct(check: Int): Int {
        var check = check
        var res = 1
        while (check > 0) {
            res *= check % 10
            check = check / 10
        }
        return res
    }
}
```