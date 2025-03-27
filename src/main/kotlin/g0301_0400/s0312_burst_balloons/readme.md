[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 312\. Burst Balloons

Hard

You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the <code>i<sup>th</sup></code> balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return _the maximum coins you can collect by bursting the balloons wisely_.

**Example 1:**

**Input:** nums = [3,1,5,8]

**Output:** 167

**Explanation:** nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> [] coins = 3\*1\*5 + 3\*5\*8 + 1\*3\*8 + 1\*8\*1 = 167

**Example 2:**

**Input:** nums = [1,5]

**Output:** 10

**Constraints:**

*   `n == nums.length`
*   `1 <= n <= 300`
*   `0 <= nums[i] <= 100`

## Solution

```kotlin
class Solution {
    fun maxCoins(nums: IntArray): Int {
        if (nums.size == 0) {
            return 0
        }
        val dp = Array(nums.size) { IntArray(nums.size) }
        return balloonBurstDp(nums, dp)
    }

    private fun balloonBurstDp(nums: IntArray, dp: Array<IntArray>): Int {
        for (gap in nums.indices) {
            var si = 0
            var ei = gap
            while (ei < nums.size) {
                val l = if (si - 1 == -1) 1 else nums[si - 1]
                val r = if (ei + 1 == nums.size) 1 else nums[ei + 1]
                var maxAns = (-1e7).toInt()
                for (cut in si..ei) {
                    val leftAns = if (si == cut) 0 else dp[si][cut - 1]
                    val rightAns = if (ei == cut) 0 else dp[cut + 1][ei]
                    val myAns = leftAns + l * nums[cut] * r + rightAns
                    maxAns = Math.max(maxAns, myAns)
                }
                dp[si][ei] = maxAns
                si++
                ei++
            }
        }
        return dp[0][nums.size - 1]
    }
}
```