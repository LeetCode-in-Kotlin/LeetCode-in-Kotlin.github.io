[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3105\. Longest Strictly Increasing or Strictly Decreasing Subarray

Easy

You are given an array of integers `nums`. Return _the length of the **longest** subarray of_ `nums` _which is either **strictly increasing** or **strictly decreasing**_.

**Example 1:**

**Input:** nums = [1,4,3,3,2]

**Output:** 2

**Explanation:**

The strictly increasing subarrays of `nums` are `[1]`, `[2]`, `[3]`, `[3]`, `[4]`, and `[1,4]`.

The strictly decreasing subarrays of `nums` are `[1]`, `[2]`, `[3]`, `[3]`, `[4]`, `[3,2]`, and `[4,3]`.

Hence, we return `2`.

**Example 2:**

**Input:** nums = [3,3,3,3]

**Output:** 1

**Explanation:**

The strictly increasing subarrays of `nums` are `[3]`, `[3]`, `[3]`, and `[3]`.

The strictly decreasing subarrays of `nums` are `[3]`, `[3]`, `[3]`, and `[3]`.

Hence, we return `1`.

**Example 3:**

**Input:** nums = [3,2,1]

**Output:** 3

**Explanation:**

The strictly increasing subarrays of `nums` are `[3]`, `[2]`, and `[1]`.

The strictly decreasing subarrays of `nums` are `[3]`, `[2]`, `[1]`, `[3,2]`, `[2,1]`, and `[3,2,1]`.

Hence, we return `3`.

**Constraints:**

*   `1 <= nums.length <= 50`
*   `1 <= nums[i] <= 50`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun longestMonotonicSubarray(nums: IntArray): Int {
        var inc = 1
        var dec = 1
        var res = 1
        for (i in 1 until nums.size) {
            if (nums[i] > nums[i - 1]) {
                inc += 1
                dec = 1
            } else if (nums[i] < nums[i - 1]) {
                dec += 1
                inc = 1
            } else {
                inc = 1
                dec = 1
            }
            res = max(res, max(inc, dec))
        }
        return res
    }
}
```