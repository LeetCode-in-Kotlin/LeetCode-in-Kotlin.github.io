[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3097\. Shortest Subarray With OR at Least K II

Medium

You are given an array `nums` of **non-negative** integers and an integer `k`.

An array is called **special** if the bitwise `OR` of all of its elements is **at least** `k`.

Return _the length of the **shortest** **special** **non-empty** subarray of_ `nums`, _or return_ `-1` _if no special subarray exists_.

**Example 1:**

**Input:** nums = [1,2,3], k = 2

**Output:** 1

**Explanation:**

The subarray `[3]` has `OR` value of `3`. Hence, we return `1`.

**Example 2:**

**Input:** nums = [2,1,8], k = 10

**Output:** 3

**Explanation:**

The subarray `[2,1,8]` has `OR` value of `11`. Hence, we return `3`.

**Example 3:**

**Input:** nums = [1,2], k = 0

**Output:** 1

**Explanation:**

The subarray `[1]` has `OR` value of `1`. Hence, we return `1`.

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>
*   <code>0 <= k <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minimumSubarrayLength(nums: IntArray, k: Int): Int {
        val n = nums.size
        if (nums[0] >= k) {
            return 1
        }
        var res = Int.MAX_VALUE
        for (i in 1 until n) {
            if (nums[i] >= k) {
                return 1
            }
            var j = i - 1
            while (j >= 0 && (nums[i] or nums[j]) != nums[j]) {
                nums[j] = nums[j] or nums[i]
                if (nums[j] >= k) {
                    res = min(res, (i - j + 1))
                }
                j--
            }
        }
        if (res == Int.MAX_VALUE) {
            return -1
        }
        return res
    }
}
```