[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2547\. Minimum Cost to Split an Array

Hard

You are given an integer array `nums` and an integer `k`.

Split the array into some number of non-empty subarrays. The **cost** of a split is the sum of the **importance value** of each subarray in the split.

Let `trimmed(subarray)` be the version of the subarray where all numbers which appear only once are removed.

*   For example, `trimmed([3,1,2,4,3,4]) = [3,4,3,4].`

The **importance value** of a subarray is `k + trimmed(subarray).length`.

*   For example, if a subarray is `[1,2,3,3,3,4,4]`, then trimmed(`[1,2,3,3,3,4,4]) = [3,3,3,4,4].`The importance value of this subarray will be `k + 5`.

Return _the minimum possible cost of a split of_ `nums`.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,2,1,2,1,3,3], k = 2

**Output:** 8

**Explanation:** We split nums to have two subarrays: [1,2], [1,2,1,3,3]. '

The importance value of [1,2] is 2 + (0) = 2. 

The importance value of [1,2,1,3,3] is 2 + (2 + 2) = 6. 

The cost of the split is 2 + 6 = 8. It can be shown that this is the minimum possible cost among all the possible splits.

**Example 2:**

**Input:** nums = [1,2,1,2,1], k = 2

**Output:** 6

**Explanation:** We split nums to have two subarrays: [1,2], [1,2,1]. 

The importance value of [1,2] is 2 + (0) = 2. 

The importance value of [1,2,1] is 2 + (2) = 4. 

The cost of the split is 2 + 4 = 6. It can be shown that this is the minimum possible cost among all the possible splits.

**Example 3:**

**Input:** nums = [1,2,1,2,1], k = 5

**Output:** 10

**Explanation:** We split nums to have one subarray: [1,2,1,2,1]. 

The importance value of [1,2,1,2,1] is 5 + (3 + 2) = 10. 

The cost of the split is 10. It can be shown that this is the minimum possible cost among all the possible splits.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `0 <= nums[i] < nums.length`
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun minCost(nums: IntArray, k: Int): Int {
        val n = nums.size
        val dp = IntArray(n)
        dp.fill(-1)
        val len = Array(n) { IntArray(n) }
        for (r in len) r.fill(0)
        for (i in 0 until n) {
            val count = IntArray(n)
            count.fill(0)
            var c = 0
            for (j in i until n) {
                count[nums[j]] += 1
                if (count[nums[j]] == 2) c += 2 else if (count[nums[j]] > 2) c += 1
                len[i][j] = c
            }
        }
        return f(0, nums, k, len, dp)
    }

    private fun f(ind: Int, nums: IntArray, k: Int, len: Array<IntArray>, dp: IntArray): Int {
        if (ind >= nums.size) return 0
        if (dp[ind] != -1) return dp[ind]
        dp[ind] = Int.MAX_VALUE
        for (i in ind until nums.size) {
            val current = len[ind][i] + k
            val next = f(i + 1, nums, k, len, dp)
            dp[ind] = dp[ind].coerceAtMost(current + next)
        }
        return dp[ind]
    }
}
```