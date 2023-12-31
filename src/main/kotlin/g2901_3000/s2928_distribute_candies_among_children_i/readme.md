[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2928\. Distribute Candies Among Children I

Easy

You are given two positive integers `n` and `limit`.

Return _the **total number** of ways to distribute_ `n` _candies among_ `3` _children such that no child gets more than_ `limit` _candies._

**Example 1:**

**Input:** n = 5, limit = 2

**Output:** 3

**Explanation:** There are 3 ways to distribute 5 candies such that no child gets more than 2 candies: (1, 2, 2), (2, 1, 2) and (2, 2, 1).

**Example 2:**

**Input:** n = 3, limit = 3

**Output:** 10

**Explanation:** There are 10 ways to distribute 3 candies such that no child gets more than 3 candies: (0, 0, 3), (0, 1, 2), (0, 2, 1), (0, 3, 0), (1, 0, 2), (1, 1, 1), (1, 2, 0), (2, 0, 1), (2, 1, 0) and (3, 0, 0).

**Constraints:**

*   `1 <= n <= 50`
*   `1 <= limit <= 50`

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun distributeCandies(n: Int, limit: Int): Int {
        var count = 0
        for (i in 0 until min(limit, n) + 1) {
            for (j in 0 until min(limit, n) + 1) {
                val k = n - i - j
                if (k >= 0 && k <= limit) {
                    count++
                }
            }
        }
        return count
    }
}
```