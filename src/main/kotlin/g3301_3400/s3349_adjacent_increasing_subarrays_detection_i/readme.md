[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3349\. Adjacent Increasing Subarrays Detection I

Easy

Given an array `nums` of `n` integers and an integer `k`, determine whether there exist **two** **adjacent** subarrays of length `k` such that both subarrays are **strictly** **increasing**. Specifically, check if there are **two** subarrays starting at indices `a` and `b` (`a < b`), where:

*   Both subarrays `nums[a..a + k - 1]` and `nums[b..b + k - 1]` are **strictly increasing**.
*   The subarrays must be **adjacent**, meaning `b = a + k`.

Return `true` if it is _possible_ to find **two** such subarrays, and `false` otherwise.

**Example 1:**

**Input:** nums = [2,5,7,8,9,2,3,4,3,1], k = 3

**Output:** true

**Explanation:**

*   The subarray starting at index `2` is `[7, 8, 9]`, which is strictly increasing.
*   The subarray starting at index `5` is `[2, 3, 4]`, which is also strictly increasing.
*   These two subarrays are adjacent, so the result is `true`.

**Example 2:**

**Input:** nums = [1,2,3,4,4,4,4,5,6,7], k = 5

**Output:** false

**Constraints:**

*   `2 <= nums.length <= 100`
*   `1 < 2 * k <= nums.length`
*   `-1000 <= nums[i] <= 1000`

## Solution

```kotlin
class Solution {
    fun hasIncreasingSubarrays(nums: List<Int>, k: Int): Boolean {
        val l = nums.size
        if (l < k * 2) {
            return false
        }
        for (i in 0..<l - 2 * k + 1) {
            if (check(i, k, nums) && check(i + k, k, nums)) {
                return true
            }
        }
        return false
    }

    private fun check(p: Int, k: Int, nums: List<Int>): Boolean {
        for (i in p..<p + k - 1) {
            if (nums[i] >= nums[i + 1]) {
                return false
            }
        }
        return true
    }
}
```