[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3095\. Shortest Subarray With OR at Least K I

Easy

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

*   `1 <= nums.length <= 50`
*   `0 <= nums[i] <= 50`
*   `0 <= k < 64`

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minimumSubarrayLength(nums: IntArray, k: Int): Int {
        val n = nums.size
        var maxL = n + 1
        var `val`: Int
        for (i in 0 until n) {
            `val` = 0
            for (j in i until n) {
                `val` = `val` or nums[j]
                if (`val` >= k) {
                    maxL = min(maxL, (j - i + 1))
                }
            }
        }
        return if (maxL == n + 1) -1 else maxL
    }
}
```