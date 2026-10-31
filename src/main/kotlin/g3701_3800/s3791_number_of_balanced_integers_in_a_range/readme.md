[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3791\. Number of Balanced Integers in a Range

Hard

You are given two integers `low` and `high`.

An integer is called **balanced** if it satisfies **both** of the following conditions:

*   It contains **at least** two digits.
*   The **sum of digits at even positions** is equal to the **sum of digits at odd positions** (the leftmost digit has position 1).

Return an integer representing the number of balanced integers in the range `[low, high]` (both inclusive).

**Example 1:**

**Input:** low = 1, high = 100

**Output:** 9

**Explanation:**

The 9 balanced numbers between 1 and 100 are 11, 22, 33, 44, 55, 66, 77, 88, and 99.

**Example 2:**

**Input:** low = 120, high = 129

**Output:** 1

**Explanation:**

Only 121 is balanced because the sum of digits at even and odd positions are both 2.

**Example 3:**

**Input:** low = 1234, high = 1234

**Output:** 0

**Explanation:**

1234 is not balanced because the sum of digits at odd positions `(1 + 3 = 4)` does not equal the sum at even positions `(2 + 4 = 6)`.

**Constraints:**

*   <code>1 <= low <= high <= 10<sup>15</sup></code>

## Solution

```kotlin
class Solution {
    private fun solve(number: String, idx: Int, diff: Int, tight: Int, dp: Array<Array<LongArray>>): Long {
        if (idx == number.length) {
            return (if (diff == 0) 1 else 0).toLong()
        }
        if (dp[idx][diff + dp[0].size / 2][tight] != -1L) {
            return dp[idx][diff + dp[0].size / 2][tight]
        }
        var ans: Long = 0
        val ub = if (tight == 1) number[idx].code - '0'.code else 9
        for (i in 0..ub) {
            val newTight = if (tight == 1 && i == ub) 1 else 0
            if (idx % 2 == 0) {
                ans += solve(number, idx + 1, diff - i, newTight, dp)
            } else {
                ans += solve(number, idx + 1, diff + i, newTight, dp)
            }
        }
        dp[idx][diff + dp[0].size / 2][tight] = ans
        return dp[idx][diff + dp[0].size / 2][tight]
    }

    private fun solve(num: Long): Long {
        if (num == 0L) {
            return 1
        }
        val number = num.toString()
        val len = number.length
        val dp: Array<Array<LongArray>> =
            if (len % 2 ==
                1
            ) {
                Array<Array<LongArray>>(len) { Array<LongArray>((len + 1) * 9 + 1) { LongArray(2) } }
            } else {
                Array<Array<LongArray>>(
                    len,
                ) { Array<LongArray>(len * 9 + 1) { LongArray(2) } }
            }
        for (tbl in dp) {
            for (row in tbl) {
                row.fill(-1)
            }
        }
        return solve(number, 0, 0, 1, dp)
    }

    fun countBalanced(low: Long, high: Long): Long = solve(high) - solve(low - 1)
}
```