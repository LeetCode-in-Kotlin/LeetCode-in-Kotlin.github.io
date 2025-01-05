[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3405\. Count the Number of Arrays with K Matching Adjacent Elements

Hard

You are given three integers `n`, `m`, `k`. A **good array** `arr` of size `n` is defined as follows:

*   Each element in `arr` is in the **inclusive** range `[1, m]`.
*   _Exactly_ `k` indices `i` (where `1 <= i < n`) satisfy the condition `arr[i - 1] == arr[i]`.

Return the number of **good arrays** that can be formed.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** n = 3, m = 2, k = 1

**Output:** 4

**Explanation:**

*   There are 4 good arrays. They are `[1, 1, 2]`, `[1, 2, 2]`, `[2, 1, 1]` and `[2, 2, 1]`.
*   Hence, the answer is 4.

**Example 2:**

**Input:** n = 4, m = 2, k = 2

**Output:** 6

**Explanation:**

*   The good arrays are `[1, 1, 1, 2]`, `[1, 1, 2, 2]`, `[1, 2, 2, 2]`, `[2, 1, 1, 1]`, `[2, 2, 1, 1]` and `[2, 2, 2, 1]`.
*   Hence, the answer is 6.

**Example 3:**

**Input:** n = 5, m = 2, k = 0

**Output:** 2

**Explanation:**

*   The good arrays are `[1, 2, 1, 2, 1]` and `[2, 1, 2, 1, 2]`. Hence, the answer is 2.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= m <= 10<sup>5</sup></code>
*   `0 <= k <= n - 1`

## Solution

```kotlin
class Solution {
    fun countGoodArrays(n: Int, m: Int, k: Int): Int {
        val f = LongArray(n + 1)
        f[0] = 1
        f[1] = 1
        for (i in 2..<f.size) {
            f[i] = f[i - 1] * i % MOD
        }
        var ans = comb(n - 1, k, f)
        ans = ans * m % MOD
        ans = ans * ex(m - 1L, n - k - 1L) % MOD
        return ans.toInt()
    }

    private fun ex(b: Long, e: Long): Long {
        var b = b
        var e = e
        var ans: Long = 1
        while (e > 0) {
            if (e % 2 == 1L) {
                ans = (ans * b) % MOD
            }
            b = (b * b) % MOD
            e = e shr 1
        }
        return ans
    }

    private fun comb(n: Int, r: Int, f: LongArray): Long {
        return f[n] * ff(f[r]) % MOD * ff(f[n - r]) % MOD
    }

    private fun ff(x: Long): Long {
        return ex(x, MOD - 2L)
    }

    companion object {
        private val MOD = (1e9 + 7).toInt()
    }
}
```