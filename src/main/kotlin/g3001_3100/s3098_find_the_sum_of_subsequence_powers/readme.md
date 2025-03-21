[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3098\. Find the Sum of Subsequence Powers

Hard

You are given an integer array `nums` of length `n`, and a **positive** integer `k`.

The **power** of a subsequence is defined as the **minimum** absolute difference between **any** two elements in the subsequence.

Return _the **sum** of **powers** of **all** subsequences of_ `nums` _which have length_ **_equal to_** `k`.

Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [1,2,3,4], k = 3

**Output:** 4

**Explanation:**

There are 4 subsequences in `nums` which have length 3: `[1,2,3]`, `[1,3,4]`, `[1,2,4]`, and `[2,3,4]`. The sum of powers is `|2 - 3| + |3 - 4| + |2 - 1| + |3 - 4| = 4`.

**Example 2:**

**Input:** nums = [2,2], k = 2

**Output:** 0

**Explanation:**

The only subsequence in `nums` which has length 2 is `[2,2]`. The sum of powers is `|2 - 2| = 0`.

**Example 3:**

**Input:** nums = [4,3,-1], k = 2

**Output:** 10

**Explanation:**

There are 3 subsequences in `nums` which have length 2: `[4,3]`, `[4,-1]`, and `[3,-1]`. The sum of powers is `|4 - 3| + |4 - (-1)| + |3 - (-1)| = 10`.

**Constraints:**

*   `2 <= n == nums.length <= 50`
*   <code>-10<sup>8</sup> <= nums[i] <= 10<sup>8</sup></code>
*   `2 <= k <= n`

## Solution

```kotlin
import kotlin.math.min

class Solution {
    private var len = 0

    private fun dfs(lastIdx: Int, k: Int, minDiff: Int, dp: MutableMap<Long, Int>, nums: IntArray): Int {
        if (k == 0) {
            return minDiff
        }
        val key = ((minDiff.toLong()) shl 12) + (lastIdx.toLong() shl 6) + k
        if (dp.containsKey(key)) {
            return dp[key]!!
        }
        var res = 0
        for (i in lastIdx + 1..len - k) {
            res = (
                res + dfs(
                    i, k - 1, min(minDiff, (nums[i] - nums[lastIdx])), dp, nums,
                )
                ) % MOD
        }
        dp[key] = res
        return res
    }

    fun sumOfPowers(nums: IntArray, k: Int): Int {
        len = nums.size
        nums.sort()
        val dp: MutableMap<Long, Int> = HashMap()
        var res = 0
        for (i in 0..len - k) {
            res = (res + dfs(i, k - 1, nums[len - 1] - nums[0], dp, nums)) % MOD
        }
        return res
    }

    companion object {
        private const val MOD = 1000000007
    }
}
```