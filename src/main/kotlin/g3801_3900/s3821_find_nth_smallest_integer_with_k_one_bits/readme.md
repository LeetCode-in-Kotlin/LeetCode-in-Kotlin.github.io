[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3821\. Find Nth Smallest Integer With K One Bits

Hard

You are given two positive integers `n` and `k`.

Return an integer denoting the <code>n<sup>th</sup></code> smallest positive integer that has **exactly** `k` ones in its binary representation. It is guaranteed that the answer is **strictly less** than <code>2<sup>50</sup></code>.

**Example 1:**

**Input:** n = 4, k = 2

**Output:** 9

**Explanation:**

The 4 smallest positive integers that have exactly `k = 2` ones in their binary representations are:

*   <code>3 = 11<sub>2</sub></code>
*   <code>5 = 101<sub>2</sub></code>
*   <code>6 = 110<sub>2</sub></code>
*   <code>9 = 1001<sub>2</sub></code>

**Example 2:**

**Input:** n = 3, k = 1

**Output:** 4

**Explanation:**

The 3 smallest positive integers that have exactly `k = 1` one in their binary representations are:

*   <code>1 = 1<sub>2</sub></code>
*   <code>2 = 10<sub>2</sub></code>
*   <code>4 = 100<sub>2</sub></code>

**Constraints:**

*   <code>1 <= n <= 2<sup>50</sup></code>
*   `1 <= k <= 50`
*   The answer is strictly less than <code>2<sup>50</sup></code>.

## Solution

```kotlin
class Solution {
    fun nthSmallest(n: Long, k: Int): Long {
        var n = n
        var k = k
        var ans: Long = 0
        for (i in 50 downTo 0) {
            val count: Long = C[i][k]
            if (n > count) {
                n -= count
                ans = ans or (1L shl i)
                k--
                if (k == 0) {
                    break
                }
            }
        }
        return ans
    }

    companion object {
        private const val MX = 60
        private val C = Array<LongArray>(MX) { LongArray(MX + 1) }

        init {
            for (i in 0..<MX) {
                C[i][0] = 1
                for (j in 1..i) {
                    C[i][j] = C[i - 1][j - 1] + C[i - 1][j]
                }
            }
        }
    }
}
```