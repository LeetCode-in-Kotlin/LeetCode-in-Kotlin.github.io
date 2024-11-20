[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2799\. Count Complete Subarrays in an Array

Medium

You are given an array `nums` consisting of **positive** integers.

We call a subarray of an array **complete** if the following condition is satisfied:

*   The number of **distinct** elements in the subarray is equal to the number of distinct elements in the whole array.

Return _the number of **complete** subarrays_.

A **subarray** is a contiguous non-empty part of an array.

**Example 1:**

**Input:** nums = [1,3,1,2,2]

**Output:** 4

**Explanation:** The complete subarrays are the following: [1,3,1,2], [1,3,1,2,2], [3,1,2] and [3,1,2,2]. 

**Example 2:**

**Input:** nums = [5,5,5,5]

**Output:** 10

**Explanation:** The array consists only of the integer 5, so any subarray is complete. The number of subarrays that we can choose is 10. 

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `1 <= nums[i] <= 2000`

## Solution

```kotlin
class Solution {
    fun countCompleteSubarrays(nums: IntArray): Int {
        val n = nums.size
        var map = IntArray(2001)
        var distinct = 0
        var last = 0
        for (i in 0 until n) {
            map[nums[i]]++
            if (map[nums[i]] == 1) {
                distinct++
                last = i
            }
        }
        map = IntArray(2001)
        for (i in 0..last) map[nums[i]]++
        var ans = 0
        for (i in 0 until n) {
            ans += n - last
            map[nums[i]]--
            if (map[nums[i]] == 0) {
                var possLast = 0
                var j = last + 1
                while (j < n && map[nums[i]] == 0) {
                    map[nums[j]]++
                    possLast = j
                    ++j
                }
                last = if (map[nums[i]] > 0) {
                    possLast
                } else {
                    break
                }
            }
        }
        return ans
    }
}
```