[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1671\. Minimum Number of Removals to Make Mountain Array

Hard

You may recall that an array `arr` is a **mountain array** if and only if:

*   `arr.length >= 3`
*   There exists some index `i` (**0-indexed**) with `0 < i < arr.length - 1` such that:
    *   `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
    *   `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

Given an integer array `nums`, return _the **minimum** number of elements to remove to make_ `nums` _a **mountain array**._

**Example 1:**

**Input:** nums = [1,3,1]

**Output:** 0

**Explanation:** The array itself is a mountain array so we do not need to remove any elements.

**Example 2:**

**Input:** nums = [2,1,1,5,6,2,3,1]

**Output:** 3

**Explanation:** One solution is to remove the elements at indices 0, 1, and 5, making the array nums = [1,5,6,3,1].

**Constraints:**

*   `3 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   It is guaranteed that you can make a mountain array out of `nums`.

## Solution

```kotlin
class Solution {
    fun minimumMountainRemovals(nums: IntArray): Int {
        val n = nums.size
        // lbs -> longest bitomic subsequence
        var lbs = 0
        val dp = IntArray(n)
        // dp[i] -> lis end at index i, dp2[i] -> lds end at index i
        val dp2 = IntArray(n)
        var lis: MutableList<Int> = ArrayList()
        // calculate longest increasing subsequence
        for (i in 0 until n - 1) {
            if (lis.isEmpty() || lis[lis.size - 1] < nums[i]) {
                lis.add(nums[i])
            } else {
                val idx = lis.binarySearch(nums[i])
                if (idx < 0) {
                    lis[-idx - 1] = nums[i]
                }
            }
            dp[i] = lis.size
        }
        lis = ArrayList()
        // calculate longest decreasing subsequence
        for (i in n - 1 downTo 1) {
            if (lis.isEmpty() || lis[lis.size - 1] < nums[i]) {
                lis.add(nums[i])
            } else {
                val idx = lis.binarySearch(nums[i])
                if (idx < 0) {
                    lis[-idx - 1] = nums[i]
                }
            }
            dp2[i] = lis.size
            if (dp[i] > 1 && dp2[i] > 1) {
                lbs = Math.max(lbs, dp[i] + dp2[i] - 1)
            }
        }
        return n - lbs
    }
}
```