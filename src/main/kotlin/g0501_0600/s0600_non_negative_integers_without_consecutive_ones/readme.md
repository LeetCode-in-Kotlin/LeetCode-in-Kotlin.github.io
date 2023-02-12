[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 600\. Non-negative Integers without Consecutive Ones

Hard

Given a positive integer `n`, return the number of the integers in the range `[0, n]` whose binary representations **do not** contain consecutive ones.

**Example 1:**

**Input:** n = 5

**Output:** 5

**Explanation:**

    Here are the non-negative integers <= 5 with their corresponding binary representations:
    0 : 0
    1 : 1
    2 : 10
    3 : 11
    4 : 100
    5 : 101
    Among them, only integer 3 disobeys the rule (two consecutive ones) and the other 5 satisfy the rule. 

**Example 2:**

**Input:** n = 1

**Output:** 2

**Example 3:**

**Input:** n = 2

**Output:** 3

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun findIntegers(n: Int): Int {
        val f = IntArray(32)
        f[0] = 1
        f[1] = 2
        var ans = 0
        var preBit = 0
        for (i in 2..31) {
            f[i] = f[i - 1] + f[i - 2]
        }
        for (k in 31 downTo 0) {
            if (n and (1 shl k) != 0) {
                // if that bit is on
                ans += f[k]
                if (preBit != 0) {
                    return ans
                }
                preBit = 1
            } else {
                preBit = 0
            }
        }
        return ans + 1
    }
}
```