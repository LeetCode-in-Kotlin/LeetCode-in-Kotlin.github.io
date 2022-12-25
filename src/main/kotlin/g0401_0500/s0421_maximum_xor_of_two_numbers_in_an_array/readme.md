[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 421\. Maximum XOR of Two Numbers in an Array

Medium

Given an integer array `nums`, return _the maximum result of_ `nums[i] XOR nums[j]`, where `0 <= i <= j < n`.

**Example 1:**

**Input:** nums = [3,10,5,25,2,8]

**Output:** 28

**Explanation:** The maximum result is 5 XOR 25 = 28.

**Example 2:**

**Input:** nums = [14,70,53,83,49,91,36,80,92,51,66,70]

**Output:** 127

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
class Solution {
    fun findMaximumXOR(nums: IntArray): Int {
        var max = 0
        var mask = 0
        val set: MutableSet<Int> = HashSet()
        var maxNum = 0
        for (i in nums) {
            maxNum = Math.max(maxNum, i)
        }
        for (i in 31 - Integer.numberOfLeadingZeros(maxNum) downTo 0) {
            set.clear()
            val bit = 1 shl i
            mask = mask or bit
            val temp = max or bit
            for (prefix in nums) {
                if (set.contains(prefix and mask xor temp)) {
                    max = temp
                    break
                }
                set.add(prefix and mask)
            }
        }
        return max
    }
}
```