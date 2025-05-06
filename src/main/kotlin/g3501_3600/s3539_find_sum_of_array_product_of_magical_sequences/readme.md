[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3539\. Find Sum of Array Product of Magical Sequences

Hard

You are given two integers, `m` and `k`, and an integer array `nums`.

A sequence of integers `seq` is called **magical** if:

*   `seq` has a size of `m`.
*   `0 <= seq[i] < nums.length`
*   The **binary representation** of <code>2<sup>seq[0]</sup> + 2<sup>seq[1]</sup> + ... + 2<sup>seq[m - 1]</sup></code> has `k` **set bits**.

The **array product** of this sequence is defined as `prod(seq) = (nums[seq[0]] * nums[seq[1]] * ... * nums[seq[m - 1]])`.

Return the **sum** of the **array products** for all valid **magical** sequences.

Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **set bit** refers to a bit in the binary representation of a number that has a value of 1.

**Example 1:**

**Input:** m = 5, k = 5, nums = [1,10,100,10000,1000000]

**Output:** 991600007

**Explanation:**

All permutations of `[0, 1, 2, 3, 4]` are magical sequences, each with an array product of 10<sup>13</sup>.

**Example 2:**

**Input:** m = 2, k = 2, nums = [5,4,3,2,1]

**Output:** 170

**Explanation:**

The magical sequences are `[0, 1]`, `[0, 2]`, `[0, 3]`, `[0, 4]`, `[1, 0]`, `[1, 2]`, `[1, 3]`, `[1, 4]`, `[2, 0]`, `[2, 1]`, `[2, 3]`, `[2, 4]`, `[3, 0]`, `[3, 1]`, `[3, 2]`, `[3, 4]`, `[4, 0]`, `[4, 1]`, `[4, 2]`, and `[4, 3]`.

**Example 3:**

**Input:** m = 1, k = 1, nums = [28]

**Output:** 28

**Explanation:**

The only magical sequence is `[0]`.

**Constraints:**

*   `1 <= k <= m <= 30`
*   `1 <= nums.length <= 50`
*   <code>1 <= nums[i] <= 10<sup>8</sup></code>

## Solution

```kotlin
class Solution {
    fun magicalSum(m: Int, k: Int, nums: IntArray): Int {
        val n = nums.size
        val pow = Array<LongArray>(n) { LongArray(m + 1) }
        for (j in 0..<n) {
            pow[j][0] = 1L
            for (c in 1..m) {
                pow[j][c] = pow[j][c - 1] * nums[j] % MOD
            }
        }
        var dp = Array<Array<LongArray>>(m + 1) { Array<LongArray>(k + 1) { LongArray(m + 1) } }
        var next = Array<Array<LongArray>>(m + 1) { Array<LongArray>(k + 1) { LongArray(m + 1) } }
        dp[0][0][0] = 1L
        for (i in 0..<n) {
            for (t in 0..m) {
                for (o in 0..k) {
                    next[t][o].fill(0L)
                }
            }
            for (t in 0..m) {
                for (o in 0..k) {
                    for (c in 0..m) {
                        if (dp[t][o][c] == 0L) {
                            continue
                        }
                        for (cc in 0..m - t) {
                            val total = c + cc
                            if (o + (total and 1) > k) {
                                continue
                            }
                            next[t + cc][o + (total and 1)][total ushr 1] =
                                (
                                    (
                                        next[t + cc][o + (total and 1)][total ushr 1] +
                                            dp[t][o][c] *
                                            C[m - t][cc] %
                                            MOD
                                            * pow[i][cc] %
                                            MOD
                                        ) %
                                        MOD
                                    )
                        }
                    }
                }
            }
            val tmp = dp
            dp = next
            next = tmp
        }
        var res: Long = 0
        for (o in 0..k) {
            for (c in 0..m) {
                if (o + P[c] == k) {
                    res = (res + dp[m][o][c]) % MOD
                }
            }
        }
        return res.toInt()
    }

    companion object {
        private const val MOD = 1000000007
        private val C: Array<IntArray> = precomputeBinom(31)
        private val P: IntArray = precomputePop(31)

        private fun precomputeBinom(max: Int): Array<IntArray> {
            val res = Array<IntArray>(max) { IntArray(max) }
            for (i in 0..<max) {
                res[i][i] = 1
                res[i][0] = res[i][i]
                for (j in 1..<i) {
                    res[i][j] = (res[i - 1][j - 1] + res[i - 1][j]) % MOD
                }
            }
            return res
        }

        private fun precomputePop(max: Int): IntArray {
            val res = IntArray(max)
            for (i in 1..<max) {
                res[i] = res[i shr 1] + (i and 1)
            }
            return res
        }
    }
}
```