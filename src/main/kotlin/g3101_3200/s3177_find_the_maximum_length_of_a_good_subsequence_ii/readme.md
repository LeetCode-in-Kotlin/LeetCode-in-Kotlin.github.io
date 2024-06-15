[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3177\. Find the Maximum Length of a Good Subsequence II

Hard

You are given an integer array `nums` and a **non-negative** integer `k`. A sequence of integers `seq` is called **good** if there are **at most** `k` indices `i` in the range `[0, seq.length - 2]` such that `seq[i] != seq[i + 1]`.

Return the **maximum** possible length of a **good** subsequence of `nums`.

**Example 1:**

**Input:** nums = [1,2,1,1,3], k = 2

**Output:** 4

**Explanation:**

The maximum length subsequence is <code>[<ins>1</ins>,<ins>2</ins>,<ins>1</ins>,<ins>1</ins>,3]</code>.

**Example 2:**

**Input:** nums = [1,2,3,4,5,1], k = 0

**Output:** 2

**Explanation:**

The maximum length subsequence is <code>[<ins>1</ins>,2,3,4,5,<ins>1</ins>]</code>.

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>3</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `0 <= k <= min(50, nums.length)`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximumLength(nums: IntArray, k: Int): Int {
        val hm = HashMap<Int, Int>()
        val n = nums.size
        val pre = IntArray(n)
        for (i in 0 until n) {
            pre[i] = hm.getOrDefault(nums[i], -1)
            hm[nums[i]] = i
        }
        val dp = Array(k + 1) { IntArray(n) }
        for (i in 0 until n) {
            dp[0][i] = 1
            if (pre[i] >= 0) {
                dp[0][i] = dp[0][pre[i]] + 1
            }
        }
        for (i in 1..k) {
            var max = 0
            for (j in 0 until n) {
                if (pre[j] >= 0) {
                    dp[i][j] = dp[i][pre[j]] + 1
                }
                dp[i][j] = max(dp[i][j], (max + 1))
                max = max(max, dp[i - 1][j])
            }
        }
        var max = 0
        for (i in 0 until n) {
            max = max(max, dp[k][i])
        }
        return max
    }
}
```