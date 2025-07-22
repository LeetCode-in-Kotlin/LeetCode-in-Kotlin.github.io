[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3621\. Number of Integers With Popcount-Depth Equal to K I

Hard

You are given two integers `n` and `k`.

For any positive integer `x`, define the following sequence:

*   <code>p<sub>0</sub> = x</code>
*   <code>p<sub>i+1</sub> = popcount(p<sub>i</sub>)</code> for all `i >= 0`, where `popcount(y)` is the number of set bits (1's) in the binary representation of `y`.

This sequence will eventually reach the value 1.

The **popcount-depth** of `x` is defined as the **smallest** integer `d >= 0` such that <code>p<sub>d</sub> = 1</code>.

For example, if `x = 7` (binary representation `"111"`). Then, the sequence is: `7 → 3 → 2 → 1`, so the popcount-depth of 7 is 3.

Your task is to determine the number of integers in the range `[1, n]` whose popcount-depth is **exactly** equal to `k`.

Return the number of such integers.

**Example 1:**

**Input:** n = 4, k = 1

**Output:** 2

**Explanation:**

The following integers in the range `[1, 4]` have popcount-depth exactly equal to 1:

| x | Binary | Sequence   |
|---|--------|------------|
| 2 | `"10"` | `2 → 1`    |
| 4 | `"100"`| `4 → 1`    |

Thus, the answer is 2.

**Example 2:**

**Input:** n = 7, k = 2

**Output:** 3

**Explanation:**

The following integers in the range `[1, 7]` have popcount-depth exactly equal to 2:

| x | Binary  | Sequence       |
|---|---------|----------------|
| 3 | `"11"`  | `3 → 2 → 1`    |
| 5 | `"101"` | `5 → 2 → 1`    |
| 6 | `"110"` | `6 → 2 → 1`    |

Thus, the answer is 3.

**Constraints:**

*   <code>1 <= n <= 10<sup>15</sup></code>
*   `0 <= k <= 5`

## Solution

```kotlin
class Solution {
    companion object {
        private val comb = Array(61) { LongArray(61) }
        private val depth = IntArray(61)

        init {
            for (i in 0..60) {
                comb[i][0] = 1L
                comb[i][i] = 1L
                for (j in 1 until i) {
                    comb[i][j] = comb[i - 1][j - 1] + comb[i - 1][j]
                }
            }
            depth[1] = 0
            for (i in 2..60) {
                depth[i] = depth[i.countOneBits()] + 1
            }
        }
    }

    fun popcountDepth(n: Long, k: Int): Long {
        if (k == 0) { return 1L }
        fun countPop(x: Long, c: Int): Long {
            var res = 0L
            var ones = 0
            var bits = 0
            var t = x
            while (t > 0) {
                bits++
                t = t shr 1
            }
            for (i in bits - 1 downTo 0) {
                if ((x shr i) and 1L == 1L) {
                    val rem = c - ones
                    if (rem in 0..i) {
                        res += comb[i][rem]
                    }
                    ones++
                }
            }
            return if (ones == c) res + 1 else res
        }
        var ans = 0L
        for (c in 1..60) {
            if (depth[c] == k - 1) {
                ans += countPop(n, c)
            }
        }
        if (k == 1) { ans-- }
        return ans
    }
}
```