[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3473\. Sum of K Subarrays With Length at Least M

Medium

You are given an integer array `nums` and two integers, `k` and `m`.

Return the **maximum** sum of `k` non-overlapping subarrays of `nums`, where each subarray has a length of **at least** `m`.

**Example 1:**

**Input:** nums = [1,2,-1,3,3,4], k = 2, m = 2

**Output:** 13

**Explanation:**

The optimal choice is:

*   Subarray `nums[3..5]` with sum `3 + 3 + 4 = 10` (length is `3 >= m`).
*   Subarray `nums[0..1]` with sum `1 + 2 = 3` (length is `2 >= m`).

The total sum is `10 + 3 = 13`.

**Example 2:**

**Input:** nums = [-10,3,-1,-2], k = 4, m = 1

**Output:** \-10

**Explanation:**

The optimal choice is choosing each element as a subarray. The output is `(-10) + 3 + (-1) + (-2) = -10`.

**Constraints:**

*   `1 <= nums.length <= 2000`
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>
*   `1 <= k <= floor(nums.length / m)`
*   `1 <= m <= 3`

## Solution

```kotlin
class Solution {
    fun maxSum(nums: IntArray, k: Int, m: Int): Int {
        val n = nums.size
        val dp = Array(k + 1) { IntArray(n + 1) { Int.MIN_VALUE } }
        val ps = IntArray(n + 1)
        for (i in nums.indices) {
            ps[i + 1] = ps[i] + nums[i]
        }
        for (j in 0..n) {
            dp[0][j] = 0
        }
        for (i in 1..k) {
            var best = Int.MIN_VALUE
            for (j in (i * m)..n) {
                best = maxOf(best, dp[i - 1][j - m] - ps[j - m])
                dp[i][j] = maxOf(dp[i][j - 1], ps[j] + best)
            }
        }
        return dp[k][n]
    }
}
```