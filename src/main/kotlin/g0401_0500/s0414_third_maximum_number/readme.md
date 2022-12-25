[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 414\. Third Maximum Number

Easy

Given an integer array `nums`, return _the **third distinct maximum** number in this array. If the third maximum does not exist, return the **maximum** number_.

**Example 1:**

**Input:** nums = [3,2,1]

**Output:** 1

**Explanation:** The first distinct maximum is 3. The second distinct maximum is 2. The third distinct maximum is 1.

**Example 2:**

**Input:** nums = [1,2]

**Output:** 2

**Explanation:** The first distinct maximum is 2. The second distinct maximum is 1. The third distinct maximum does not exist, so the maximum (2) is returned instead.

**Example 3:**

**Input:** nums = [2,2,3,1]

**Output:** 1

**Explanation:** The first distinct maximum is 3. The second distinct maximum is 2 (both 2's are counted together since they have the same value). The third distinct maximum is 1.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>

**Follow up:** Can you find an `O(n)` solution?

## Solution

```kotlin
class Solution {
    fun thirdMax(nums: IntArray): Int {
        var max1 = Long.MIN_VALUE
        var max2 = Long.MIN_VALUE
        var max3 = Long.MIN_VALUE
        for (i in nums) {
            max1 = Math.max(max1, i.toLong())
        }
        for (i in nums) {
            if (i.toLong() == max1) {
                continue
            }
            max2 = Math.max(max2, i.toLong())
        }
        for (i in nums) {
            if (i.toLong() == max1 || i.toLong() == max2) {
                continue
            }
            max3 = Math.max(max3, i.toLong())
        }
        return (if (max3 == Long.MIN_VALUE) max1 else max3).toInt()
    }
}
```