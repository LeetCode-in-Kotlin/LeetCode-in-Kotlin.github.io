[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 718\. Maximum Length of Repeated Subarray

Medium

Given two integer arrays `nums1` and `nums2`, return _the maximum length of a subarray that appears in **both** arrays_.

**Example 1:**

**Input:** nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]

**Output:** 3

**Explanation:** The repeated subarray with maximum length is [3,2,1].

**Example 2:**

**Input:** nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]

**Output:** 5

**Explanation:** The repeated subarray with maximum length is [0,0,0,0,0].

**Constraints:**

*   `1 <= nums1.length, nums2.length <= 1000`
*   `0 <= nums1[i], nums2[i] <= 100`

## Solution

```kotlin
class Solution {
    fun findLength(nums1: IntArray, nums2: IntArray): Int {
        val m = nums1.size
        val n = nums2.size
        var max = 0
        val dp = Array(m + 1) { IntArray(n + 1) }
        for (i in 1..m) {
            for (j in 1..n) {
                if (nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1]
                    max = Math.max(max, dp[i][j])
                }
            }
        }
        return max
    }
}
```