[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1879\. Minimum XOR Sum of Two Arrays

Hard

You are given two integer arrays `nums1` and `nums2` of length `n`.

The **XOR sum** of the two integer arrays is `(nums1[0] XOR nums2[0]) + (nums1[1] XOR nums2[1]) + ... + (nums1[n - 1] XOR nums2[n - 1])` (**0-indexed**).

*   For example, the **XOR sum** of `[1,2,3]` and `[3,2,1]` is equal to `(1 XOR 3) + (2 XOR 2) + (3 XOR 1) = 2 + 0 + 2 = 4`.

Rearrange the elements of `nums2` such that the resulting **XOR sum** is **minimized**.

Return _the **XOR sum** after the rearrangement_.

**Example 1:**

**Input:** nums1 = [1,2], nums2 = [2,3]

**Output:** 2

**Explanation:** Rearrange `nums2` so that it becomes `[3,2]`. The XOR sum is (1 XOR 3) + (2 XOR 2) = 2 + 0 = 2.

**Example 2:**

**Input:** nums1 = [1,0,3], nums2 = [5,3,4]

**Output:** 8

**Explanation:** Rearrange `nums2` so that it becomes `[5,4,3]`. The XOR sum is (1 XOR 5) + (0 XOR 4) + (3 XOR 3) = 4 + 4 + 0 = 8.

**Constraints:**

*   `n == nums1.length`
*   `n == nums2.length`
*   `1 <= n <= 14`
*   <code>0 <= nums1[i], nums2[i] <= 10<sup>7</sup></code>

## Solution

```kotlin
class Solution {
    fun minimumXORSum(nums1: IntArray, nums2: IntArray): Int {
        val l = nums1.size
        val dp = IntArray(1 shl l)
        dp.fill(-1)
        dp[0] = 0
        return dfs(dp.size - 1, l, nums1, nums2, dp, l)
    }

    private fun dfs(state: Int, length: Int, nums1: IntArray, nums2: IntArray, dp: IntArray, totalLength: Int): Int {
        if (dp[state] >= 0) {
            return dp[state]
        }
        var min = Int.MAX_VALUE
        val currIndex = totalLength - length
        var i = 0
        var index = 0
        while (i < length) {
            if (state shr index and 1 == 1) {
                val result = dfs(state xor (1 shl index), length - 1, nums1, nums2, dp, totalLength)
                min = Math.min(min, (nums2[currIndex] xor nums1[index]) + result)
                i++
            }
            index++
        }
        dp[state] = min
        return min
    }
}
```