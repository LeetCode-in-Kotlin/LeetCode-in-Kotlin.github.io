[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3350\. Adjacent Increasing Subarrays Detection II

Medium

Given an array `nums` of `n` integers, your task is to find the **maximum** value of `k` for which there exist **two** adjacent subarrays of length `k` each, such that both subarrays are **strictly** **increasing**. Specifically, check if there are **two** subarrays of length `k` starting at indices `a` and `b` (`a < b`), where:

*   Both subarrays `nums[a..a + k - 1]` and `nums[b..b + k - 1]` are **strictly increasing**.
*   The subarrays must be **adjacent**, meaning `b = a + k`.

Return the **maximum** _possible_ value of `k`.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [2,5,7,8,9,2,3,4,3,1]

**Output:** 3

**Explanation:**

*   The subarray starting at index 2 is `[7, 8, 9]`, which is strictly increasing.
*   The subarray starting at index 5 is `[2, 3, 4]`, which is also strictly increasing.
*   These two subarrays are adjacent, and 3 is the **maximum** possible value of `k` for which two such adjacent strictly increasing subarrays exist.

**Example 2:**

**Input:** nums = [1,2,3,4,4,4,4,5,6,7]

**Output:** 2

**Explanation:**

*   The subarray starting at index 0 is `[1, 2]`, which is strictly increasing.
*   The subarray starting at index 2 is `[3, 4]`, which is also strictly increasing.
*   These two subarrays are adjacent, and 2 is the **maximum** possible value of `k` for which two such adjacent strictly increasing subarrays exist.

**Constraints:**

*   <code>2 <= nums.length <= 2 * 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maxIncreasingSubarrays(nums: List<Int>): Int {
        val n = nums.size
        val a = IntArray(n)
        for (i in 0..<n) {
            a[i] = nums[i]
        }
        var ans = 1
        var previousLen = Int.Companion.MAX_VALUE
        var i = 0
        while (i < n) {
            var j = i + 1
            while (j < n && a[j - 1] < a[j]) {
                ++j
            }
            val len = j - i
            ans = max(ans, (len / 2))
            if (previousLen != Int.Companion.MAX_VALUE) {
                ans = max(ans, min(previousLen, len))
            }
            previousLen = len
            i = j
        }
        return ans
    }
}
```