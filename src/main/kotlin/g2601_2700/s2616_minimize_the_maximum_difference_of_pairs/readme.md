[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2616\. Minimize the Maximum Difference of Pairs

Medium

You are given a **0-indexed** integer array `nums` and an integer `p`. Find `p` pairs of indices of `nums` such that the **maximum** difference amongst all the pairs is **minimized**. Also, ensure no index appears more than once amongst the `p` pairs.

Note that for a pair of elements at the index `i` and `j`, the difference of this pair is `|nums[i] - nums[j]|`, where `|x|` represents the **absolute** **value** of `x`.

Return _the **minimum** **maximum** difference among all_ `p` _pairs._ We define the maximum of an empty set to be zero.

**Example 1:**

**Input:** nums = [10,1,2,7,1,3], p = 2

**Output:** 1

**Explanation:**

The first pair is formed from the indices 1 and 4, and the second pair is formed from the indices 2 and 5.

The maximum difference is max(\|nums[1] - nums[4]\|, \|nums[2] - nums[5]\|) = max(0, 1) = 1. Therefore, we return 1.

**Example 2:**

**Input:** nums = [4,2,1,2], p = 1

**Output:** 0

**Explanation:**

Let the indices 1 and 3 form a pair. The difference of that pair is \|2 - 2\| = 0, which is the minimum we can attain.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>
*   `0 <= p <= (nums.length)/2`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private fun ispossible(nums: IntArray, p: Int, diff: Int): Boolean {
        var p = p
        val n = nums.size
        var i = 1
        while (i < n) {
            if (nums[i] - nums[i - 1] <= diff) {
                p--
                i++
            }
            i++
        }
        return p <= 0
    }

    fun minimizeMax(nums: IntArray, p: Int): Int {
        val n = nums.size
        nums.sort()
        var left = 0
        var right = nums[n - 1] - nums[0]
        var ans = right
        while (left <= right) {
            val mid = left + (right - left) / 2
            if (ispossible(nums, p, mid)) {
                ans = mid
                right = mid - 1
            } else {
                left = mid + 1
            }
        }
        return ans
    }
}
```