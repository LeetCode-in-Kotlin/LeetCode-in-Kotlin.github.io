[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2929\. Distribute Candies Among Children II

Medium

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

*   <code>1 <= n <= 10<sup>6</sup></code>
*   <code>1 <= limit <= 10<sup>6</sup></code>

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun distributeCandies(n: Int, limit: Int): Long {
        var ans: Long = 0
        for (i in 0..min(n, limit)) {
            var rem = (n - i).toLong()
            if (rem > 2 * limit) continue
            // second student
            val max = min(limit.toLong(), rem)
            // for third student
            rem -= max
            // if remain is grater than limit cant possible to arrange;
            // if(rem <= limit) than max - rem combination
            if (rem <= limit) ans = ans + max - rem + 1
        }
        return ans
    }
}
```