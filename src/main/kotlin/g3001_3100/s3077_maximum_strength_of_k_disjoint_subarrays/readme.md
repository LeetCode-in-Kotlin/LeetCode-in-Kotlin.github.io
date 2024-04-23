[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3077\. Maximum Strength of K Disjoint Subarrays

Hard

You are given a **0-indexed** array of integers `nums` of length `n`, and a **positive** **odd** integer `k`.

The strength of `x` subarrays is defined as `strength = sum[1] * x - sum[2] * (x - 1) + sum[3] * (x - 2) - sum[4] * (x - 3) + ... + sum[x] * 1` where `sum[i]` is the sum of the elements in the <code>i<sup>th</sup></code> subarray. Formally, strength is sum of <code>(-1)<sup>i+1</sup> * sum[i] * (x - i + 1)</code> over all `i`'s such that `1 <= i <= x`.

You need to select `k` **disjoint subarrays** from `nums`, such that their **strength** is **maximum**.

Return _the **maximum** possible **strength** that can be obtained_.

**Note** that the selected subarrays **don't** need to cover the entire array.

**Example 1:**

**Input:** nums = [1,2,3,-1,2], k = 3

**Output:** 22

**Explanation:** The best possible way to select 3 subarrays is: nums[0..2], nums[3..3], and nums[4..4]. The strength is (1 + 2 + 3) \* 3 - (-1) \* 2 + 2 \* 1 = 22.

**Example 2:**

**Input:** nums = [12,-2,-2,-2,-2], k = 5

**Output:** 64

**Explanation:** The only possible way to select 5 disjoint subarrays is: nums[0..0], nums[1..1], nums[2..2], nums[3..3], and nums[4..4]. The strength is 12 \* 5 - (-2) \* 4 + (-2) \* 3 - (-2) \* 2 + (-2) \* 1 = 64.

**Example 3:**

**Input:** nums = [-1,-2,-3], k = 1

**Output:** -1

**Explanation:** The best possible way to select 1 subarray is: nums[0..0]. The strength is -1.

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= n`
*   <code>1 <= n * k <= 10<sup>6</sup></code>
*   `k` is odd.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximumStrength(n: IntArray, k: Int): Long {
        if (n.size == 1) {
            return n[0].toLong()
        }
        val dp = Array(n.size) { LongArray(k) }
        dp[0][0] = k.toLong() * n[0]
        for (i in 1 until k) {
            var pm: Long = -1
            dp[i][0] = (max(0, dp[i - 1][0]) + k.toLong() * n[i])
            for (j in 1 until i) {
                dp[i][j] = (
                    max(
                        dp[i - 1][j],
                        dp[i - 1][j - 1]
                    ) + (k.toLong() - j) * n[i] * pm
                    )
                pm = -pm
            }
            dp[i][i] = dp[i - 1][i - 1] + (k.toLong() - i) * n[i] * pm
        }
        var max = dp[k - 1][k - 1]
        for (i in k until n.size) {
            var pm: Long = 1
            dp[i][0] = (max(0, dp[i - 1][0]) + k.toLong() * n[i])
            for (j in 1 until k) {
                pm = -pm
                dp[i][j] = (
                    max(
                        dp[i - 1][j],
                        dp[i - 1][j - 1]
                    ) + (k.toLong() - j) * n[i] * pm
                    )
            }
            if (max < dp[i][k - 1]) {
                max = dp[i][k - 1]
            }
        }
        return max
    }
}
```