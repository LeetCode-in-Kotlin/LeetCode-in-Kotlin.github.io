[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3881\. Direction Assignments with Exactly K Visible People

Medium

You are given three integers `n`, `pos`, and `k`.

There are `n` people standing in a line indexed from 0 to `n - 1`. Each person **independently** chooses a direction:

*   `'L'`: **visible** only to people on their **right**
*   `'R'`: **visible** only to people on their **left**

A person at index `pos` sees others as follows:

*   A person `i < pos` is visible if and only if they choose `'L'`.
*   A person `i > pos` is visible if and only if they choose `'R'`.

Return the number of possible direction assignments such that the person at index `pos` sees **exactly** `k` people.

Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 3, pos = 1, k = 0

**Output:** 2

**Explanation:**

*   Index 0 is to the left of `pos = 1`, and index 2 is to the right of `pos = 1`.
*   To see `k = 0` people, index 0 must choose `'R'` and index 2 must choose `'L'`, keeping both invisible.
*   The person at index 1 can choose `'L'` or `'R'` since it does not affect the count. Thus, the answer is 2.

**Example 2:**

**Input:** n = 3, pos = 2, k = 1

**Output:** 4

**Explanation:**

*   Index 0 and index 1 are left of `pos = 2`, and there is no index to the right.
*   To see `k = 1` person, exactly one of index 0 or index 1 must choose `'L'`, and the other must choose `'R'`.
*   There are 2 ways to choose which index is visible from the left.
*   The person at index 2 can choose `'L'` or `'R'` since it does not affect the count. Thus, the answer is `2 + 2 = 4`.

**Example 3:**

**Input:** n = 1, pos = 0, k = 0

**Output:** 2

**Explanation:**

*   There are no indices to the left or right of `pos = 0`.
*   To see `k = 0` people, no additional condition is required.
*   The person at index 0 can choose `'L'` or `'R'`. Thus, the answer is 2.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `0 <= pos, k <= n - 1`

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun countVisiblePeople(n: Int, pos: Int, k: Int): Int {
        val total = n - 1
        val combinations = nCr(total, k)
        return (2L * combinations % MOD).toInt()
    }

    private fun nCr(n: Int, r: Int): Long {
        var r = r
        r = min(r, n - r)
        var numerator: Long = 1
        var denominator: Long = 1
        for (i in 1..r) {
            numerator = numerator * (n - r + i) % MOD
            denominator = denominator * i % MOD
        }
        return numerator * modPow(denominator, MOD - 2) % MOD
    }

    private fun modPow(base: Long, exponent: Long): Long {
        var base = base
        var exponent = exponent
        var result: Long = 1
        while (exponent > 0) {
            if ((exponent and 1L) == 1L) {
                result = result * base % MOD
            }

            base = base * base % MOD
            exponent = exponent shr 1
        }
        return result
    }

    companion object {
        private const val MOD = 1000000007L
    }
}
```