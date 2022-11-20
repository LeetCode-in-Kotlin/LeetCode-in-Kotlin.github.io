[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 334\. Increasing Triplet Subsequence

Medium

Given an integer array `nums`, return `true` _if there exists a triple of indices_ `(i, j, k)` _such that_ `i < j < k` _and_ `nums[i] < nums[j] < nums[k]`. If no such indices exists, return `false`.

**Example 1:**

**Input:** nums = [1,2,3,4,5]

**Output:** true

**Explanation:** Any triplet where i < j < k is valid.

**Example 2:**

**Input:** nums = [5,4,3,2,1]

**Output:** false

**Explanation:** No triplet exists.

**Example 3:**

**Input:** nums = [2,1,5,0,4,6]

**Output:** true

**Explanation:** The triplet (3, 4, 5) is valid because nums[3] == 0 < nums[4] == 4 < nums[5] == 6.

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>5</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>

**Follow up:** Could you implement a solution that runs in `O(n)` time complexity and `O(1)` space complexity?

## Solution

```kotlin
class Solution {
    fun increasingTriplet(nums: IntArray): Boolean {
        if (nums.size == 1) {
            return false
        }
        var big: Int = nums.lastIndex
        var medium: Int? = null
        for (i in nums.lastIndex - 1 downTo 0) {
            if (nums[i] > nums[big]) {
                big = i
                continue
            } else if ((medium != null && nums[i] > nums[medium] && nums[i] < nums[big]) ||
                (medium == null && nums[i] < nums[big])
            ) {
                medium = i
                continue
            } else if (medium != null && nums[i] < nums[medium]) {
                return true
            }
        }
        return false
    }
}
```