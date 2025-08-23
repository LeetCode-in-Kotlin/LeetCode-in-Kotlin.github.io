[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3653\. XOR After Range Multiplication Queries I

Medium

You are given an integer array `nums` of length `n` and a 2D integer array `queries` of size `q`, where <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>, k<sub>i</sub>, v<sub>i</sub>]</code>.

For each query, you must apply the following operations in order:

*   Set <code>idx = l<sub>i</sub></code>.
*   While <code>idx <= r<sub>i</sub></code>:
    *   Update: <code>nums[idx] = (nums[idx] * v<sub>i</sub>) % (10<sup>9</sup> + 7)</code>
    *   Set <code>idx += k<sub>i</sub></code>.

Return the **bitwise XOR** of all elements in `nums` after processing all queries.

**Example 1:**

**Input:** nums = [1,1,1], queries = \[\[0,2,1,4]]

**Output:** 4

**Explanation:**

*   A single query `[0, 2, 1, 4]` multiplies every element from index 0 through index 2 by 4.
*   The array changes from `[1, 1, 1]` to `[4, 4, 4]`.
*   The XOR of all elements is `4 ^ 4 ^ 4 = 4`.

**Example 2:**

**Input:** nums = [2,3,1,5,4], queries = \[\[1,4,2,3],[0,2,1,2]]

**Output:** 31

**Explanation:**

*   The first query `[1, 4, 2, 3]` multiplies the elements at indices 1 and 3 by 3, transforming the array to `[2, 9, 1, 15, 4]`.
*   The second query `[0, 2, 1, 2]` multiplies the elements at indices 0, 1, and 2 by 2, resulting in `[4, 18, 2, 15, 4]`.
*   Finally, the XOR of all elements is `4 ^ 18 ^ 2 ^ 15 ^ 4 = 31`.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>3</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= q == queries.length <= 10<sup>3</sup></code>
*   <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>, k<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= l<sub>i</sub> <= r<sub>i</sub> < n</code>
*   <code>1 <= k<sub>i</sub> <= n</code>
*   <code>1 <= v<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    private fun modPow(a0: Long, e0: Long): Long {
        var a = a0 % MOD
        var e = e0
        var res = 1L
        while (e > 0) {
            if ((e and 1L) == 1L) {
                res = (res * a) % MOD
            }
            a = (a * a) % MOD
            e = e shr 1
        }
        return res
    }

    private fun modInv(a: Long): Long {
        return modPow(a, MOD - 2L)
    }

    fun xorAfterQueries(nums: IntArray, queries: Array<IntArray>): Int {
        val n = nums.size
        val b = kotlin.math.sqrt(n.toDouble()).toInt()
        val small = HashMap<Int, Array<LongArray?>>()

        for (query in queries) {
            val l = query[0]
            val r = query[1]
            val k = query[2]
            val v = query[3]
            if (k > b) {
                var i = l
                while (i <= r) {
                    nums[i] = ((nums[i].toLong() * v.toLong()) % MOD).toInt()
                    i += k
                }
            } else {
                val byResidue = small.getOrPut(k) { arrayOfNulls<LongArray>(k) }
                val res = l % k
                if (byResidue[res] == null) {
                    val len = (n - res + k - 1) / k
                    byResidue[res] = LongArray(len + 1) { 1L }
                }
                val diff = byResidue[res]!!
                val jStart = (l - res) / k
                val jEnd = (r - res) / k
                diff[jStart] = (diff[jStart] * v.toLong()) % MOD
                if (jEnd + 1 < diff.size) {
                    diff[jEnd + 1] = (diff[jEnd + 1] * modInv(v.toLong())) % MOD
                }
            }
        }
        for ((k, byResidue) in small) {
            for (res in 0 until k) {
                val diff = byResidue[res] ?: continue
                var mul = 1L
                for (j in 0 until diff.size - 1) {
                    mul = (mul * diff[j]) % MOD
                    val idx = res + j * k
                    if (idx < n) {
                        nums[idx] = ((nums[idx].toLong() * mul) % MOD).toInt()
                    }
                }
            }
        }
        var ans = 0
        for (x in nums) {
            ans = ans xor x
        }
        return ans
    }

    companion object {
        private const val MOD = 1000000007L
    }
}
```