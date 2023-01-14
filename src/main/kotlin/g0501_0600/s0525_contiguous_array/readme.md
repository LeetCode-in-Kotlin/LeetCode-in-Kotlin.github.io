[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 525\. Contiguous Array

Medium

Given a binary array `nums`, return _the maximum length of a contiguous subarray with an equal number of_ `0` _and_ `1`.

**Example 1:**

**Input:** nums = [0,1]

**Output:** 2

**Explanation:** [0, 1] is the longest contiguous subarray with an equal number of 0 and 1.

**Example 2:**

**Input:** nums = [0,1,0]

**Output:** 2

**Explanation:** [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is either `0` or `1`.

## Solution

```kotlin
class Solution {
    fun findMaxLength(nums: IntArray): Int {
        for (i in nums.indices) {
            if (nums[i] == 0) {
                nums[i] = -1
            }
        }
        val map: HashMap<Int, Int> = HashMap()
        map[0] = -1
        var ps = 0
        var len = 0
        for (i in nums.indices) {
            ps += nums[i]
            if (!map.containsKey(ps)) {
                map[ps] = i
            } else {
                len = Math.max(len, i - map[ps]!!)
            }
        }
        return len
    }
}
```