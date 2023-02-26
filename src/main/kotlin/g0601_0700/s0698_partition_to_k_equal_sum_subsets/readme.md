[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 698\. Partition to K Equal Sum Subsets

Medium

Given an integer array `nums` and an integer `k`, return `true` if it is possible to divide this array into `k` non-empty subsets whose sums are all equal.

**Example 1:**

**Input:** nums = [4,3,2,3,5,2,1], k = 4

**Output:** true

**Explanation:** It is possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.

**Example 2:**

**Input:** nums = [1,2,3,4], k = 3

**Output:** false

**Constraints:**

*   `1 <= k <= nums.length <= 16`
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>
*   The frequency of each element is in the range `[1, 4]`.

## Solution

```kotlin
import java.util.Arrays

class Solution {
    fun canPartitionKSubsets(nums: IntArray, k: Int): Boolean {
        if (nums.isEmpty()) {
            return false
        }
        val n = nums.size
        var sum = 0
        for (num in nums) {
            sum += num
        }
        if (sum % k != 0) {
            return false
        }
        // sum of each subset = sum / k
        sum /= k
        val dp = IntArray(1 shl n)
        Arrays.fill(dp, -1)
        dp[0] = 0
        for (i in 0 until (1 shl n)) {
            if (dp[i] == -1) {
                continue
            }
            val rem = sum - dp[i] % sum
            for (j in 0 until n) {
                // bitmask
                val tmp = i or (1 shl j)
                // skip if the bit is already taken
                if (tmp != i) {
                    // num too big for current subset
                    if (nums[j] > rem) {
                        break
                    }
                    // cumulative sum
                    dp[tmp] = dp[i] + nums[j]
                }
            }
        }
        // true if total sum of all nums is the same
        return dp[(1 shl n) - 1] == k * sum
    }
}
```