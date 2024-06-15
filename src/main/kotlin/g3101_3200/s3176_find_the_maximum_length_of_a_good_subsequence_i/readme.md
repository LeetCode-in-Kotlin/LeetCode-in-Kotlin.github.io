[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3176\. Find the Maximum Length of a Good Subsequence I

Medium

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

*   `1 <= nums.length <= 500`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `0 <= k <= min(nums.length, 25)`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximumLength(nums: IntArray, k: Int): Int {
        val n = nums.size
        var count = 0
        for (i in 0 until nums.size - 1) {
            if (nums[i] != nums[i + 1]) {
                count++
            }
        }
        if (count <= k) {
            return n
        }
        val max = IntArray(k + 1)
        max.fill(1)
        val vis = IntArray(n)
        vis.fill(-1)
        val map = HashMap<Int, Int>()
        for (i in 0 until n) {
            if (!map.containsKey(nums[i])) {
                map[nums[i]] = i + 1
            } else {
                vis[i] = map[nums[i]]!! - 1
                map[nums[i]] = i + 1
            }
        }
        val dp = Array(n) { IntArray(k + 1) }
        for (i in 0 until n) {
            for (j in 0..k) {
                dp[i][j] = 1
            }
        }
        for (i in 1 until n) {
            for (j in k - 1 downTo 0) {
                dp[i][j + 1] = max(dp[i][j + 1], (1 + max[j]))
                max[j + 1] = max(max[j + 1], dp[i][j + 1])
            }
            if (vis[i] != -1) {
                val a = vis[i]
                for (j in 0..k) {
                    dp[i][j] = max(dp[i][j], (1 + dp[a][j]))
                    max[j] = max(dp[i][j], max[j])
                }
            }
        }
        var ans = 1
        for (i in 0..k) {
            ans = max(ans, max[i])
        }
        return ans
    }
}
```