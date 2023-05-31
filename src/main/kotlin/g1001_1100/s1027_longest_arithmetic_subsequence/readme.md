[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1027\. Longest Arithmetic Subsequence

Medium

Given an array `nums` of integers, return _the length of the longest arithmetic subsequence in_ `nums`.

**Note** that:

*   A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.
*   A sequence `seq` is arithmetic if `seq[i + 1] - seq[i]` are all the same value (for `0 <= i < seq.length - 1`).

**Example 1:**

**Input:** nums = [3,6,9,12]

**Output:** 4 **Explanation: ** The whole array is an arithmetic sequence with steps of length = 3.

**Example 2:**

**Input:** nums = [9,4,7,2,10]

**Output:** 3 **Explanation: ** The longest arithmetic subsequence is [4,7,10].

**Example 3:**

**Input:** nums = [20,1,15,3,10,5,8]

**Output:** 4 **Explanation: ** The longest arithmetic subsequence is [20,15,10,5].

**Constraints:**

*   `2 <= nums.length <= 1000`
*   `0 <= nums[i] <= 500`

## Solution

```kotlin
import java.util.Arrays

class Solution {
    fun longestArithSeqLength(nums: IntArray): Int {
        val max = maxElement(nums)
        val min = minElement(nums)
        val diff = max - min
        val n = nums.size
        val dp = Array(n) { IntArray(2 * diff + 2) }
        for (d in dp) {
            Arrays.fill(d, 1)
        }
        var ans = 0
        for (i in 0 until n) {
            for (j in i - 1 downTo 0) {
                val difference = nums[i] - nums[j] + diff
                val temp = dp[j][difference]
                dp[i][difference] = Math.max(dp[i][difference], temp + 1)
                if (ans < dp[i][difference]) {
                    ans = dp[i][difference]
                }
            }
        }
        return ans
    }

    private fun maxElement(arr: IntArray): Int {
        var max = Int.MIN_VALUE
        for (e in arr) {
            if (max < e) {
                max = e
            }
        }
        return max
    }

    private fun minElement(arr: IntArray): Int {
        var min = Int.MAX_VALUE
        for (e in arr) {
            if (min > e) {
                min = e
            }
        }
        return min
    }
}
```