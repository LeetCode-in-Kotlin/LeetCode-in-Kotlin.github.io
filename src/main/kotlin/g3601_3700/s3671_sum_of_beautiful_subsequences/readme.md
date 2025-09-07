[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3671\. Sum of Beautiful Subsequences

Hard

You are given an integer array `nums` of length `n`.

For every **positive** integer `g`, we define the **beauty** of `g` as the **product** of `g` and the number of **strictly increasing** **subsequences** of `nums` whose greatest common divisor (GCD) is exactly `g`.

Return the **sum** of **beauty** values for all positive integers `g`.

Since the answer could be very large, return it modulo <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 10

**Explanation:**

All strictly increasing subsequences and their GCDs are:

| Subsequence | GCD |
|-------------|-----|
| [1]         | 1   |
| [2]         | 2   |
| [3]         | 3   |
| [1,2]       | 1   |
| [1,3]       | 1   |
| [2,3]       | 1   |
| [1,2,3]     | 1   |

Calculating beauty for each GCD:

| GCD | Count of subsequences | Beauty (GCD × Count)  |
|-----|------------------------|----------------------|
| 1   | 5                      | 1 × 5 = 5            |
| 2   | 1                      | 2 × 1 = 2            |
| 3   | 1                      | 3 × 1 = 3            |

Total beauty is `5 + 2 + 3 = 10`.

**Example 2:**

**Input:** nums = [4,6]

**Output:** 12

**Explanation:**

All strictly increasing subsequences and their GCDs are:

| Subsequence | GCD |
|-------------|-----|
| [4]         | 4   |
| [6]         | 6   |
| [4,6]       | 2   |

Calculating beauty for each GCD:

| GCD | Count of subsequences | Beauty (GCD × Count) |
|-----|------------------------|----------------------|
| 2   | 1                      | 2 × 1 = 2            |
| 4   | 1                      | 4 × 1 = 4            |
| 6   | 1                      | 6 × 1 = 6            |

Total beauty is `2 + 4 + 6 = 12`.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 7 * 10<sup>4</sup></code>

## Solution

```kotlin
import kotlin.math.sqrt

class Solution {
    fun totalBeauty(nums: IntArray): Int {
        var maxV = 0
        for (v in nums) {
            if (v > maxV) {
                maxV = v
            }
        }
        // index by g
        val fenwicks = arrayOfNulls<Fenwick>(maxV + 1)
        // FDiv[g] = # inc subseq with all elements multiple of g
        val fDiv = LongArray(maxV + 1)
        // temp buffer for divisors (max divisors of any number <= ~128 for this constraint)
        val divisors = IntArray(256)
        // Left-to-right DP restricted to multiples of each divisor g
        for (x in nums) {
            var cnt = 0
            val r = sqrt(x.toDouble()).toInt()
            for (d in 1..r) {
                if (x % d == 0) {
                    divisors[cnt++] = d
                    val d2 = x / d
                    if (d2 != d) {
                        divisors[cnt++] = d2
                    }
                }
            }
            for (i in 0..<cnt) {
                val g = divisors[i]
                // coordinate in [1..maxV/g] for this g
                val idxQ = x / g
                var fw = fenwicks[g]
                if (fw == null) {
                    // Size needs to be >= max index (maxV/g). Use +2 for safety and 1-based
                    // indexing.
                    fw = Fenwick(maxV / g + 2)
                    fenwicks[g] = fw
                }
                var dp = 1 + fw.query(idxQ - 1)
                if (dp >= MOD) {
                    dp -= MOD.toLong()
                }
                fw.add(idxQ, dp)
                fDiv[g] += dp
                if (fDiv[g] >= MOD) {
                    fDiv[g] -= MOD.toLong()
                }
            }
        }
        // Inclusion–exclusion to get exact gcd counts
        val exact = LongArray(maxV + 1)
        for (g in maxV downTo 1) {
            var s = fDiv[g]
            var m = g + g
            while (m <= maxV) {
                s -= exact[m]
                if (s < 0) {
                    s += MOD.toLong()
                }
                m += g
            }
            exact[g] = s
        }
        var ans: Long = 0
        for (g in 1..maxV) {
            if (exact[g] != 0L) {
                ans += exact[g] * g % MOD
                if (ans >= MOD) {
                    ans -= MOD.toLong()
                }
            }
        }
        return ans.toInt()
    }

    private class Fenwick(size: Int) {
        private val tree: LongArray = LongArray(size)

        fun add(indexOneBased: Int, delta: Long) {
            var i = indexOneBased
            while (i < tree.size) {
                var v = tree[i] + delta
                if (v >= MOD) {
                    v -= MOD.toLong()
                }
                tree[i] = v
                i += i and -i
            }
        }

        fun query(indexOneBased: Int): Long {
            var sum: Long = 0
            var i = indexOneBased
            while (i > 0) {
                sum += tree[i]
                if (sum >= MOD) {
                    sum -= MOD.toLong()
                }
                i -= i and -i
            }
            return sum
        }
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```