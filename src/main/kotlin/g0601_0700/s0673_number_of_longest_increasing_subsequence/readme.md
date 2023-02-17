[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 673\. Number of Longest Increasing Subsequence

Medium

Given an integer array `nums`, return _the number of longest increasing subsequences._

**Notice** that the sequence has to be **strictly** increasing.

**Example 1:**

**Input:** nums = [1,3,5,4,7]

**Output:** 2

**Explanation:** The two longest increasing subsequences are [1, 3, 4, 7] and [1, 3, 5, 7].

**Example 2:**

**Input:** nums = [2,2,2,2,2]

**Output:** 5

**Explanation:** The length of the longest increasing subsequence is 1, and there are 5 increasing subsequences of length 1, so output 5.

**Constraints:**

*   `1 <= nums.length <= 2000`
*   <code>-10<sup>6</sup> <= nums[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun findNumberOfLIS(nums: IntArray): Int {
        val dp = IntArray(nums.size)
        val count = IntArray(nums.size)
        dp[0] = 1
        count[0] = 1
        var result = 0
        var max = Int.MIN_VALUE
        for (i in 1 until nums.size) {
            dp[i] = 1
            count[i] = 1
            for (j in i - 1 downTo 0) {
                if (nums[j] < nums[i]) {
                    if (dp[i] < dp[j] + 1) {
                        dp[i] = dp[j] + 1
                        count[i] = count[j]
                    } else if (dp[i] == dp[j] + 1) {
                        count[i] += count[j]
                    }
                }
            }
        }
        for (i in nums.indices) {
            if (max < dp[i]) {
                result = count[i]
                max = dp[i]
            } else if (max == dp[i]) {
                result += count[i]
            }
        }
        return result
    }
}
```