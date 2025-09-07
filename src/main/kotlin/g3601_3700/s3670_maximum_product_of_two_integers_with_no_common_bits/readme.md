[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3670\. Maximum Product of Two Integers With No Common Bits

Medium

You are given an integer array `nums`.

Your task is to find two **distinct** indices `i` and `j` such that the product `nums[i] * nums[j]` is **maximized,** and the binary representations of `nums[i]` and `nums[j]` do not share any common set bits.

Return the **maximum** possible product of such a pair. If no such pair exists, return 0.

**Example 1:**

**Input:** nums = [1,2,3,4,5,6,7]

**Output:** 12

**Explanation:**

The best pair is 3 (011) and 4 (100). They share no set bits and `3 * 4 = 12`.

**Example 2:**

**Input:** nums = [5,6,4]

**Output:** 0

**Explanation:**

Every pair of numbers has at least one common set bit. Hence, the answer is 0.

**Example 3:**

**Input:** nums = [64,8,32]

**Output:** 2048

**Explanation:**

No pair of numbers share a common bit, so the answer is the product of the two maximum elements, 64 and 32 (`64 * 32 = 2048`).

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
class Solution {
    fun maxProduct(nums: IntArray): Long {
        // Find highest value to limit DP size
        var maxVal = 0
        for (v in nums) {
            if (v > maxVal) {
                maxVal = v
            }
        }
        // If all numbers are >=1, maxVal > 0; compute needed bit-width
        // in [1..20]
        val maxBits = 32 - Integer.numberOfLeadingZeros(maxVal)
        val size = 1 shl maxBits
        // ---- store input midway, as required ----
        // dp[mask] = largest number present whose bitmask == mask (later becomes: max over all
        // submasks)
        val dp = IntArray(size)
        for (x in nums) {
            // numbers themselves are their masks
            if (dp[x] < x) {
                dp[x] = x
            }
        }
        // SOS DP: for each bit b, propagate lower-half block maxima to upper-half block
        // (branch-light)
        for (b in 0..<maxBits) {
            val half = 1 shl b
            val step = half shl 1
            var base = 0
            while (base < size) {
                val upper = base + half
                for (m in 0..<half) {
                    val u = upper + m
                    val l = base + m
                    if (dp[u] < dp[l]) {
                        dp[u] = dp[l]
                    }
                }
                base += step
            }
        }
        // Now dp[mask] = max value among all submasks of 'mask'
        var ans: Long = 0
        val full = size - 1
        for (x in nums) {
            // masks with no bits in common with x
            val complement = (x.inv()) and full
            // best partner disjoint with x
            val y = dp[complement]
            if (y > 0) {
                val prod = x.toLong() * y
                if (prod > ans) {
                    ans = prod
                }
            }
        }
        // 0 if no valid pair
        return ans
    }
}
```