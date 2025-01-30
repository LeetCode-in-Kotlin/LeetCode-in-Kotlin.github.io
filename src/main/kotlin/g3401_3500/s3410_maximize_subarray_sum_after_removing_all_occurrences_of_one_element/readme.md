[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3410\. Maximize Subarray Sum After Removing All Occurrences of One Element

Hard

You are given an integer array `nums`.

You can do the following operation on the array **at most** once:

*   Choose **any** integer `x` such that `nums` remains **non-empty** on removing all occurrences of `x`.
*   Remove **all** occurrences of `x` from the array.

Return the **maximum** subarray sum across **all** possible resulting arrays.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [-3,2,-2,-1,3,-2,3]

**Output:** 7

**Explanation:**

We can have the following arrays after at most one operation:

*   The original array is <code>nums = [-3, 2, -2, -1, <ins>**3, -2, 3**</ins>]</code>. The maximum subarray sum is `3 + (-2) + 3 = 4`.
*   Deleting all occurences of `x = -3` results in <code>nums = [2, -2, -1, **<ins>3, -2, 3</ins>**]</code>. The maximum subarray sum is `3 + (-2) + 3 = 4`.
*   Deleting all occurences of `x = -2` results in <code>nums = [-3, **<ins>2, -1, 3, 3</ins>**]</code>. The maximum subarray sum is `2 + (-1) + 3 + 3 = 7`.
*   Deleting all occurences of `x = -1` results in <code>nums = [-3, 2, -2, **<ins>3, -2, 3</ins>**]</code>. The maximum subarray sum is `3 + (-2) + 3 = 4`.
*   Deleting all occurences of `x = 3` results in <code>nums = [-3, <ins>**2**</ins>, -2, -1, -2]</code>. The maximum subarray sum is 2.

The output is `max(4, 4, 7, 4, 2) = 7`.

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** 10

**Explanation:**

It is optimal to not perform any operations.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>6</sup> <= nums[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maxSubarraySum(nums: IntArray): Long {
        val prefixMap: MutableMap<Long, Long> = HashMap<Long, Long>()
        var result = nums[0].toLong()
        var prefixSum: Long = 0
        var minPrefix: Long = 0
        prefixMap.put(0L, 0L)
        for (num in nums) {
            prefixSum += num.toLong()
            result = max(result, (prefixSum - minPrefix))
            if (num < 0) {
                if (prefixMap.containsKey(num.toLong())) {
                    prefixMap.put(
                        num.toLong(),
                        min(prefixMap[num.toLong()]!!, prefixMap[0L]!!) + num,
                    )
                } else {
                    prefixMap.put(num.toLong(), prefixMap[0L]!! + num)
                }
                minPrefix = min(minPrefix, prefixMap[num.toLong()]!!)
            }
            prefixMap.put(0L, min(prefixMap[0L]!!, prefixSum))
            minPrefix = min(minPrefix, prefixMap[0L]!!)
        }
        return result
    }
}
```