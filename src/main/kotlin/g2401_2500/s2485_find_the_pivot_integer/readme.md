[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2485\. Find the Pivot Integer

Easy

Given a positive integer `n`, find the **pivot integer** `x` such that:

*   The sum of all elements between `1` and `x` inclusively equals the sum of all elements between `x` and `n` inclusively.

Return _the pivot integer_ `x`. If no such integer exists, return `-1`. It is guaranteed that there will be at most one pivot index for the given input.

**Example 1:**

**Input:** n = 8

**Output:** 6

**Explanation:** 6 is the pivot integer since: 1 + 2 + 3 + 4 + 5 + 6 = 6 + 7 + 8 = 21.

**Example 2:**

**Input:** n = 1

**Output:** 1

**Explanation:** 1 is the pivot integer since: 1 = 1.

**Example 3:**

**Input:** n = 4

**Output:** -1

**Explanation:** It can be proved that no such integer exist.

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```kotlin
class Solution {
    fun pivotInteger(n: Int): Int {
        if (n == 0 || n == 1) {
            return n
        }
        var sum = 0
        for (i in 1..n) {
            sum += i
        }
        var ad = 0
        for (i in 1..n) {
            ad += i - 1
            sum -= i
            if (sum == ad) {
                return i
            }
        }
        return -1
    }
}
```