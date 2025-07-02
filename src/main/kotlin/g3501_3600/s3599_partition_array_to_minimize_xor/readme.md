[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3599\. Partition Array to Minimize XOR

Medium

You are given an integer array `nums` and an integer `k`.

Your task is to partition `nums` into `k` non-empty ****non-empty subarrays****. For each subarray, compute the bitwise **XOR** of all its elements.

Return the **minimum** possible value of the **maximum XOR** among these `k` subarrays.

**Example 1:**

**Input:** nums = [1,2,3], k = 2

**Output:** 1

**Explanation:**

The optimal partition is `[1]` and `[2, 3]`.

*   XOR of the first subarray is `1`.
*   XOR of the second subarray is `2 XOR 3 = 1`.

The maximum XOR among the subarrays is 1, which is the minimum possible.

**Example 2:**

**Input:** nums = [2,3,3,2], k = 3

**Output:** 2

**Explanation:**

The optimal partition is `[2]`, `[3, 3]`, and `[2]`.

*   XOR of the first subarray is `2`.
*   XOR of the second subarray is `3 XOR 3 = 0`.
*   XOR of the third subarray is `2`.

The maximum XOR among the subarrays is 2, which is the minimum possible.

**Example 3:**

**Input:** nums = [1,1,2,3,1], k = 2

**Output:** 0

**Explanation:**

The optimal partition is `[1, 1]` and `[2, 3, 1]`.

*   XOR of the first subarray is `1 XOR 1 = 0`.
*   XOR of the second subarray is `2 XOR 3 XOR 1 = 0`.

The maximum XOR among the subarrays is 0, which is the minimum possible.

**Constraints:**

*   `1 <= nums.length <= 250`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= n`

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun minXor(nums: IntArray, k: Int): Int {
        val n = nums.size
        // Step 1: Prefix XOR array
        val pfix = IntArray(n + 1)
        for (i in 1..n) {
            pfix[i] = pfix[i - 1] xor nums[i - 1]
        }
        // Step 2: DP table
        val dp: Array<IntArray> = Array(n + 1) { IntArray(k + 1) }
        for (row in dp) {
            row.fill(Int.Companion.MAX_VALUE)
        }
        for (i in 0..n) {
            // Base case: 1 partition
            dp[i][1] = pfix[i]
        }
        // Step 3: Fill DP for partitions 2 to k
        for (parts in 2..k) {
            for (end in parts..n) {
                for (split in parts - 1..<end) {
                    val segmentXOR = pfix[end] xor pfix[split]
                    val maxXOR = max(dp[split][parts - 1], segmentXOR)
                    dp[end][parts] = min(dp[end][parts], maxXOR)
                }
            }
        }
        return dp[n][k]
    }
}
```