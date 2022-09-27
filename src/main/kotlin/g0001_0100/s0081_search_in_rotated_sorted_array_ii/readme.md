[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 81\. Search in Rotated Sorted Array II

Medium

There is an integer array `nums` sorted in non-decreasing order (not necessarily with **distinct** values).

Before being passed to your function, `nums` is **rotated** at an unknown pivot index `k` (`0 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,4,4,5,6,6,7]` might be rotated at pivot index `5` and become `[4,5,6,6,7,0,1,2,4,4]`.

Given the array `nums` **after** the rotation and an integer `target`, return `true` _if_ `target` _is in_ `nums`_, or_ `false` _if it is not in_ `nums`_._

You must decrease the overall operation steps as much as possible.

**Example 1:**

**Input:** nums = [2,5,6,0,0,1,2], target = 0

**Output:** true

**Example 2:**

**Input:** nums = [2,5,6,0,0,1,2], target = 3

**Output:** false

**Constraints:**

*   `1 <= nums.length <= 5000`
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>
*   `nums` is guaranteed to be rotated at some pivot.
*   <code>-10<sup>4</sup> <= target <= 10<sup>4</sup></code>

**Follow up:** This problem is similar to [Search in Rotated Sorted Array](/problems/search-in-rotated-sorted-array/description/), but `nums` may contain **duplicates**. Would this affect the runtime complexity? How and why?

## Solution

```kotlin
class Solution {
    fun search(nums: IntArray, target: Int): Boolean {
        return binary(nums, 0, nums.size - 1, target)
    }

    private fun binary(a: IntArray, i: Int, j: Int, t: Int): Boolean {
        if (i > j) {
            return false
        }
        val mid = (i + j) / 2
        if (a[mid] == t) {
            return true
        }
        val c1 = binary(a, i, mid - 1, t)
        val c2 = binary(a, mid + 1, j, t)
        return c1 || c2
    }
}
```