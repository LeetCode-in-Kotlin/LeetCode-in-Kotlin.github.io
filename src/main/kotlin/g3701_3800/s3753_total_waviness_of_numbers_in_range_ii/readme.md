[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3753\. Total Waviness of Numbers in Range II

Hard

You are given two integers `num1` and `num2` representing an **inclusive** range `[num1, num2]`.

The **waviness** of a number is defined as the total count of its **peaks** and **valleys**:

*   A digit is a **peak** if it is **strictly greater** than both of its immediate neighbors.
*   A digit is a **valley** if it is **strictly less** than both of its immediate neighbors.
*   The first and last digits of a number **cannot** be peaks or valleys.
*   Any number with fewer than 3 digits has a waviness of 0.

Return the total sum of waviness for all numbers in the range `[num1, num2]`.

**Example 1:**

**Input:** num1 = 120, num2 = 130

**Output:** 3

**Explanation:**

In the range `[120, 130]`:

*   `120`: middle digit 2 is a peak, waviness = 1.
*   `121`: middle digit 2 is a peak, waviness = 1.
*   `130`: middle digit 3 is a peak, waviness = 1.
*   All other numbers in the range have a waviness of 0.

Thus, total waviness is `1 + 1 + 1 = 3`.

**Example 2:**

**Input:** num1 = 198, num2 = 202

**Output:** 3

**Explanation:**

In the range `[198, 202]`:

*   `198`: middle digit 9 is a peak, waviness = 1.
*   `201`: middle digit 0 is a valley, waviness = 1.
*   `202`: middle digit 0 is a valley, waviness = 1.
*   All other numbers in the range have a waviness of 0.

Thus, total waviness is `1 + 1 + 1 = 3`.

**Example 3:**

**Input:** num1 = 4848, num2 = 4848

**Output:** 2

**Explanation:**

Number `4848`: the second digit 8 is a peak, and the third digit 4 is a valley, giving a waviness of 2.

**Constraints:**

*   <code>1 <= num1 <= num2 <= 10<sup>15</sup></code>

## Solution

```kotlin
class Solution {
    private class Pair(var count: Long, var sum: Long)

    private lateinit var digits: CharArray
    private lateinit var memo: Array<Array<Array<Array<Array<Pair?>>>>>
    private lateinit var seen: Array<Array<Array<Array<BooleanArray>>>> // Changed innermost to BooleanArray

    fun totalWaviness(num1: Long, num2: Long): Long = solve(num2) - solve(num1 - 1)

    private fun solve(x: Long): Long {
        if (x <= 0) {
            return 0
        }
        digits = x.toString().toCharArray()
        val n = digits.size
        memo = Array(n) { Array(2) { Array(2) { Array(11) { arrayOfNulls<Pair>(11) } } } }
        seen = Array(n) { Array(2) { Array(2) { Array(11) { BooleanArray(11) } } } }
        return dfs(0, 1, 0, 10, 10).sum
    }

    private fun dfs(pos: Int, tight: Int, started: Int, prev2: Int, prev1: Int): Pair {
        if (pos == digits.size) {
            return Pair(1, 0)
        }
        if (seen[pos][tight][started][prev2][prev1]) {
            return memo[pos][tight][started][prev2][prev1]!!
        }
        seen[pos][tight][started][prev2][prev1] = true

        val limit = if (tight == 1) digits[pos] - '0' else 9
        var totalCount = 0L
        var totalSum = 0L

        for (d in 0..limit) {
            val nextTight = if (tight == 1 && d == limit) 1 else 0
            when {
                started == 0 && d == 0 -> {
                    val nxt = dfs(pos + 1, nextTight, 0, 10, 10)
                    totalCount += nxt.count
                    totalSum += nxt.sum
                }

                started == 0 -> {
                    val nxt = dfs(pos + 1, nextTight, 1, 10, d)
                    totalCount += nxt.count
                    totalSum += nxt.sum
                }

                prev2 == 10 -> {
                    val nxt = dfs(pos + 1, nextTight, 1, prev1, d)
                    totalCount += nxt.count
                    totalSum += nxt.sum
                }

                else -> {
                    var add = 0
                    if ((prev1 > prev2 && prev1 > d) || (prev1 < prev2 && prev1 < d)) {
                        add = 1
                    }
                    val nxt = dfs(pos + 1, nextTight, 1, prev1, d)
                    totalCount += nxt.count
                    totalSum += nxt.sum + add * nxt.count
                }
            }
        }
        val result = Pair(totalCount, totalSum)
        memo[pos][tight][started][prev2][prev1] = result
        return result
    }
}
```